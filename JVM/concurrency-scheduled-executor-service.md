`ExecutorService` 을 편리하게 생성해주는 정적 팩터리 메서드들이 모여있는 `Executors` 유틸리티를 살펴보니, `ScheduledExecutorService` 타입의 반환형도 있었다. 이름에서 알 수 있듯이, 제출한 `Runnable`/`Callable` 의 실행시기를 조절할 수 있는 기능을 제공해준다. 간단한 예제를 작성해봤다.

```java
// 간단한 Runnable 을 준비한다.
// 1초 자고 일어나서 thread 이름과 현재 시각을 출력한다.
Runnable runnable = ()->{
    try {
        Thread.sleep(1000);
        System.out.println(Thread.currentThread().getName() + " : " + System.currentTimeMillis());
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
};

// 크기 3짜리 thread pool 을 생성
ScheduledExecutorService service =
        Executors.newScheduledThreadPool(3, new ThreadFactoryBuilder().setNameFormat("Study-%d").build());

// 일부러 thread pool 크기보다 하나 더 많이 Runnable 을 제출한다. 
service.schedule(runnable, 1L, TimeUnit.SECONDS);
System.out.println("runnable scheduled");
service.schedule(runnable, 1L, TimeUnit.SECONDS);
System.out.println("runnable scheduled");
service.schedule(runnable, 1L, TimeUnit.SECONDS);
System.out.println("runnable scheduled");
service.schedule(runnable, 1L, TimeUnit.SECONDS);
System.out.println("runnable scheduled");

service.shutdown();
service.awaitTermination(Long.MAX_VALUE, TimeUnit.MILLISECONDS);
``` 
```shell
Study-1 : 1482940364310
Study-2 : 1482940364310
Study-0 : 1482940364311
Study-1 : 1482940365377
```

앞 Runnable 3개는 거의 동시에 실행되고, 나머지 Runnable 은 대략 1초뒤에 실행되는걸 확인했다. console 출력문을 보면, thread pool 크기가 3이지만 schedule 메서드가 연달아 4번 실행되는데는 문제가 없었다. 하지만 1번 thread 에서 두 Runnable 을 순차적으로 실행할 수 밖에 없으므로, `Thread.sleep(1000)` 하는 시간만큼 마지막 Runnable 이 대략 1000 ms 정도 밀린 것이다.

이외에도 schedule 기준을 **"실행 간 간격"** 으로 할 것인지, **"종료 후 실행까지 기간"** 으로 할 것인지 선택할 수 있는 두 메서드가 제공된다. 아주 유용할 것이다. 