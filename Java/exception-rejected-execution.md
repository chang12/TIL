thread pool 을 만들고, `Runnable` 을 submit 하는 코드를 작성하면, checked exception 이 없으므로 try-catch 문을 적지 않고 넘어가게 된다. 그러나 `AbstractExecutorService` 추상 클래스에 구현된 submit 메서드의 주석을 읽어보니 `RuntimeException` 들을 던지고 있었다. 

요새 작업중인 worker 코드에서는 while 문 안에서 예외가 발생해서 코드가 중간에 중단되거나, 반복문에서 빠뜨리는 원소가 있는 경우 골치아파지므로 RuntimeException 까지 신경써줘야겠다는 생각이 들었다. 그 중 하나인 **RejectedExecutionException** 을 간단히 정리해봤다.

```java
// thread pool 의 크기보다, 제출하는 Runnable 의 개수를 훨씬 많이 설정합니다.
// Rejection 을 유도하기 위해서입니다.
final int numOfThreads = 3;
final int numOfRunnables = 15;

ExecutorService service = new ThreadPoolExecutor(
        numOfThreads, numOfThreads,
        0L, TimeUnit.MILLISECONDS,
        new ArrayBlockingQueue<Runnable>(numOfThreads));

for (int i = 0; i < numOfRunnables; i++) {
    int finalI = i;
    try {
        service.submit(()->{
            System.out.println("submitted: " + finalI);
        });
    } catch (RejectedExecutionException e) {
        System.out.println("rejected: " + finalI);
    }
}

service.shutdown();
```   
``` shell
# 결과 출력
submitted: 0
submitted: 1
submitted: 3
submitted: 4
submitted: 5
rejected: 6
submitted: 2
submitted: 7
submitted: 8
submitted: 9
submitted: 11
submitted: 10
rejected: 12
submitted: 13
submitted: 14
```

thread pool 을 사용하는 보통의 경우, Java 라이브러리에서 제공하는 `Executors` 유틸리티 클래스의 정적 팩터리 메서드로 `ExecutorService` 인스턴스를 얻어 쓴다. 정적 팩터리 메서드의 코드를 살펴보니 크기가 조절되는 내부 `Queue<Runnable>` 를 사용하고 있었다. 

하지만 요새 작업하는 코드의 경우 만들어지는 `Runnable` 의 개수가 매우 많고, 새로운 `Runnable` 이 submit 되는 속도가 submit 된 `Runnable` 이 종료되는 속도보다 더 빠르기 때문에 `Queue<Runnable>` 이 지나치게 커지는 것이 염려되어 명시적으로 Array 로 만들었다.

그러나 Queue 에 크기제한이 생기니, 꽉찬 뒤에 `Runnable` 을 submit 하려고 시도하는 경우 이를 거부하는 `RuntimeException` 을 만나게 됬다. 그러니 이제 이를 해결하기 위한 패턴을 생각한다.