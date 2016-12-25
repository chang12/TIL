앞선 글에서 `RejectedExecutionException` 에 대해서 살펴봤었다. 상황을 간단히 정리해보자.

* `ExecutorService` 가 내부에 간직한 `BlockingQueue<Runnable>` 의 크기가 너무 커지는 것을 염려된다.
* `BlockingQueue` 를 `ArrayBlockingQueue` 로 사용하자!
* 직접 주입하기 위해 `Executors` 유틸리티 클래스가 아니라, `ThreadPoolExecutor` 생성자를 사용한다.
* `ArrayBlockingQueue` 라서 크기는 고정되지만, 꽉찬 상태에서 submit 하면 `RuntimeException` 발생.

이를 위한 첫번째 해결책을 생각해봤다. 

* `Runnable` 을 submit 하기전에 `ArrayBlockingQueue` 가 꽉찼는지 확인한다.
* 꽉찼다면 기다린다. 기다리는 시간은 1ms 로 시작해서 **exponential back-off** 규칙을 따른다.

```java
void waitUntilQueueIsAvailable(BlockingQueue<Runnable> queue) throws InterruptedException {
    long backOffMillis = 1L;
    while (queue.remainingCapacity() == 0) {
        Thread.sleep(backOffMillis);
        backOffMillis *= 2;
    }
}
```

위에서 정의한 메서드를 Thread Pool 에 Runnable 을 submit 할때마다 그전에 호출한다.