concurrent task 를 시키고 싶을때 thread pool 을 이용한다. Java 5 에서 Concurrent API 가 나오기전에는 thread pool 을 쓰기 위해서 프로그래머들이 직접 구현했다고 하니 정말 슬픈 이야기다. 요새 접하는 코드에서는 대부분의 경우 thread pool 을 쓰기 위해서 **`ExecutorService`** 구현체를 사용하고, 구현체는 **`Executors`** 의 정적 팩터리 메서드에서 받아온다.

```java
ExecutorService service = Executors.newFixedThreadPool(최대_쓰레드_개수, ThreadFactory);
ExecutorService service = Executors.newCachedThreadPool(ThreadFactory);
```

**newFixedThreadPool** 메서드는 최대 thread 개수를 보장하는 thread pool 을 만들고, **newCachedThreadPool** 메서드는 (재사용하면서 없으면) 필요할때마다 새로운 thread 를 만드는 thread pool 을 만든다.

## 의문: fixed thread pool 에 가용 thread 가 없다면?
동일한 작업을 병렬적으로 처리하기 위한 **Worker** 코드들은 대부분 비슷한 구조를 띄었다. 

* Worker 인스턴스는 run 메서드를 구현하고, 메서드 내부에서 thread pool 을 생성한다. 
* 원하는 만큼 반복문을 돌면서 원하는 작업에 대한 Runnable 을 service 에 제출한다.

여기서 의문이 들었던 부분은, fixed thread pool 의 최대 개수만큼 thread 가 만들어지고, 모든 thread 가 실행중이서 가용 thread 가 없는 상황에서 새로운 Runnable 을 제출하면 **블로킹이 될지 여부**였다. 예를 들어 원하는 작업의 반복 횟수는 100개이고, thread pool 의 크기는 10개인 상황에서 제출한 **첫 10개의 Runnable 중 하나라도 끝나기 전에 이미 11번째 Runnable 을 submit** 하는 상황을 생각했다.

이를 위해 간단한 테스트 코드를 작성해서 테스트해봤다.

```java
int sizeOfThreadPool = 10;
int numOfSubmit = 100;
AtomicInteger count = new AtomicInteger(0);

ExecutorService service =
    Executors.newFixedThreadPool(sizeOfThreadPool, new ThreadFactoryBuilder().setNameFormat("worker-thread-%d").build());

for (int i = 0; i < numOfSubmit; i++) {
    service.submit(() -> {
        String msg =
            String.format("Thread name: %s, Runnable number: %d", Thread.currentThread().getName(), count.addAndGet(1));
        System.out.println(msg);
    });
    System.out.println("submit runnable to service: " + i);
}
```
```
submit runnable to service: 0
submit runnable to service: 1
submit runnable to service: 2
submit runnable to service: 3
...
submit runnable to service: 97
submit runnable to service: 98
submit runnable to service: 99
(모든 submit 이 끝난 뒤에야 Runnable 실행 종료가 시작)
Thread name: worker-thread-2, Runnable number: 3
Thread name: worker-thread-4, Runnable number: 5
Thread name: worker-thread-3, Runnable number: 4
Thread name: worker-thread-4, Runnable number: 8
Thread name: worker-thread-1, Runnable number: 2
Thread name: worker-thread-3, Runnable number: 9
Thread name: worker-thread-4, Runnable number: 10
...
```

결과는 명백했다. 가용 thread 의 유무는 thread pool 사용자의 submit 에 영향을 미치지 않았다. 그 원인은 **`ThreadPoolExecutor`** 클래스에서 찾을 수 있었다. `BlockingQueue<Runnable>` 타입의 workQueue 필드가 있어서 우선 제출받은 `Runnable` 은 `Queue` 에 쌓아두고 가용 thread 가 생길때마다 새로운 Thread 로 만들어서 실행하는 형태였다. 그러므로 submit 이 블로킹되지 않았던 것이다.

## 추가 생각
위 테스트 코드에서는 `Runnable` 을 제출하는 쪽은 단일 thread 였다. 하지만 이 마저도 thread pool 을 사용해서 멀티 thread 로 처리할 수 있다. 지금까지 봐온 경우로는 이를 위한 thread pool 을 사용할때는 pool 크기 만큼만 Runnable(작업 Runnable 을 만들어서 작업 thread pool 에 제출하는) 을 제출한다. 

이 경우 두개의 thread pool 이 존재하고, 그 크기는 다를 수 있다. 그러므로 **두 크기를 어떻게 설정**해야 좋은 엔지니어링이 될지 고민할만한 이슈라는 생각을 했다. 