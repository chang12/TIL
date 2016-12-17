**Thread Pool** 에 관해서 탐색하다가 `InterruptedException` 의 개념을 접했다. 직관적으로 와닿지가 않아서 직접 코드를 간단히 짜서 확인해봤다.

```java
Thread slave = new Thread(new Runnable() {
    @Override
    public void run() {
        try {
            Thread.sleep(TimeUnit.MILLISECONDS.convert(10, TimeUnit.SECONDS));
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            System.out.println("slave thread has finished.");
        }
    }
});

slave.start();
slave.interrupt();
```
```console
java.lang.InterruptedException: sleep interrupted
slave thread has finished.
	at java.lang.Thread.sleep(Native Method)
	at kr.fakenerd.study.java.exceptions.InterruptedExceptionMain$1.run(InterruptedExceptionMain.java:13)
	at java.lang.Thread.run(Thread.java:745)

Process finished with exit code 0
```

sleep 상태의 thread 의 interrupt 메서드를 호출하면 `InterruptedException` 을 던진다는걸 확인할 수 있었다. 일반적으로 추측해보자면 sleep 외에도 `InterruptedException` 을 던질 수 있는 조건들이 있고, 해당 조건에 놓인 thread 의 interrupt 메서드를 호출하면 `InterruptedException` 이 던져질것이다.

interrupt 메서드는 해당 thread 의 작업을 중단시킬 목적으로 다른 thread 에 의해서 호출된다.