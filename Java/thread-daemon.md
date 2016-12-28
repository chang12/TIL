Java process 의 main thread 의 실행은 어딘가 클래스에 구현된 main 메서드의 실행을 따라간다. main thread 는 다른 thread 를 생성해서 실행시켜 process 실행을 병렬화할 수 있다. 이 경우 생성한 thread 가 **daemon 인지 여부**에 따라서, main thread 의 실행이 종료된 후 바로 process 가 종료되는지 여부가 결정된다. 

생성해서 실행한 thread 가 daemon thread 일 경우, thread 가 종료되지 않았더라도 main thread 가 종료되었다면 process 가 바로 종료된다. non-daemon thread 인 경우에는 main thread 에서 `System.exit(int)` 메서드를 호출해서 명시적으로 process 를 종료하지 않았다면, non-daemon thread 들이 모두 종료될때까지 process 는 종료되지 않는다.

thread 의 daemon 여부 설정은 `Thread` 클래스의 인스턴스 메서드 호출로 할 수 있다.

```java
Thread t = new Thread(...);
t.setDaemon(true);
```