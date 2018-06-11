transaction 에 대한 코드와 application server 의 context 쪽 코드를 보다가, `ThreadLocal` 타입의 변수가 쓰이는걸 봤다. `ThreadLocal` 의 내용을 이해해야 코드를 이해할 수 있을 것 같아서 간단히 정리해봤다. 동작을 이해하기 위해 간단히 테스트 코드를 작성했다.

```java
public final class ThreadLocalMain {
    private ThreadLocalMain() {}
    
    // thread pool 의 thread 들이 공유할 ThreadLocal 정적 변수를 초기화한다.
    private static ThreadLocal<String> local = new ThreadLocal<>();

    public static void main(String[] args) throws InterruptedException {
        int numOfThreads = 5;

        ExecutorService service
                = Executors.newFixedThreadPool(numOfThreads, new ThreadFactoryBuilder().setNameFormat("thread-%d").build());
        for (int i = 0; i < numOfThreads; i++) {
            service.submit(() -> {
                String name = Thread.currentThread().getName();
                local.set(name);
                System.out.println("thread local has set: " + name);
                try {
                    // thread pool 의 다른 thread 들도 local 값을 set 하도록 1초간 기다린다.
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(String.format("thread(%s) get value : %s", name, local.get()));
            });
        }

        service.shutdown();
        service.awaitTermination(Long.MAX_VALUE, TimeUnit.MILLISECONDS);
    }
}
``` 
```java
thread local has set: thread-0
thread local has set: thread-1
thread local has set: thread-2
thread local has set: thread-3
thread local has set: thread-4
thread(thread-0) get value : thread-0
thread(thread-3) get value : thread-3
thread(thread-2) get value : thread-2
thread(thread-4) get value : thread-4
thread(thread-1) get value : thread-1

Process finished with exit code 0
```

이름 그대로 thread local 한 지역변수를 제공해줌을 확인할 수 있었다. 같은 참조변수에 대한 set/get 메서드를 호출하더라도 호출한 thread 별로 분리된 변수가 관리되는 것이다. 

그러므로 **DB I/O** 작업을 맡은 thread에 현재 걸린 **transaction** 변수를 저장하거나, 하나의 **HTTP 요청을 처리**하는 request handler 역할의 thread 에 **context** 변수를 저장하기 위한 목적에 최적이라는 생각이 들었다. 