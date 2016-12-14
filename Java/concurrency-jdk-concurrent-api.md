이 문서에서는 JDK 5 부터 새롭게 추가된 **Concurrent API** 에 대해서 정리해본다.

http://javacan.tistory.com/entry/134

## Motivation: Web Application
웹 서버와 같이 동시에 다수의 요청을 처리해야 하는 어플리케이션을 개발해야 할 경우, 코드는 다음과 같은 형태를 띄게 된다.
```java
while(true) {
	request = acceptRequest();
	Runnable requestHandler = new RequestHandler(request);
	new Thread(requestHandler).start();
}
```
`Thread` 의 생성자에 `Runnable` 인스턴스를 넘겨주는 것은 익숙하다. `RequestHandler` 는 JDK 에 존재하는 클래스는 아니고, `Runnable` 인스턴스를 구현한 클래스일 것이다. 코드를 이해하는데는 문제가 없다. 하지만 몇가지 문제점을 지닌다.  
* 요청을 처리하는 로직이 단순한 경우에, 배보다 배꼽이 더 커질 수 있다. 실제 비즈니스 로직은 짧지만, `Thread` 생성 및 종료에 의한 오버헤드가 크기 때문이다.
* `Thread` 개수에 제한을 두지 않으므로, `OutOfMemoryError` 가 발생할 수 있다.
* `Thread` 개수가 많아지면, 스케줄링에 따른 오버헤드도 존재한다.

## Executor
위의 문제들을 해결하기 위해 동시에 다수의 요청을 처리해야하는 어플리케이션에서는 **Thread Pool** 을 사용한다. 즉, 일정 개수의 `Thread` 들을 미리 생성해놓고 그 범위내에서만 사용한다.

이 **Thread Pool** 의 개념을 아우르는 인터페이스가 `Executor` 이다. `Executor` 인터페이스는 하나의 메소드를 가진다.
```java
public interface Executor {
	// @throws RejectedExecutionException : if this task cannot be accepted for execution
	// @throws NullPointerExeception : if command is null
	void execute(Runnable command);
}
```
`Executor` 를 도입하면 기존 코드를 아래처럼 바꿀 수 있다.
```java
Executor executor = ... // Executor 구현체 인스턴스 생성
while(true) {
	request = acceptRequest();
	Runnable requestHandler = new RequestHandler(request);
	executor.execute(requestHandler);
}
```
`Thread` 인스턴스의 start 메소드를 실행할때는 `Runnable` 이 바로 실행되지만, `Executor` 인스턴스의 `execute` 메소드를 실행할때는 구체화된 로직에 따라서 얼마든지 실행 시기를 달리할 수 있다. 예를 들어 **Thread Pool** 개념의 구현이었다면 파라미터로 받은 `Runnable` 을 우선 작업 queue 에 넣고, 실제 실행은 뒤로 pending 될 것이다.
 
이를 통해 작업을 생성하는 로직과 (`RequestHandler` 구현), 작업을 실행하는 로직 (`execute` 메소드 구현) 사이의 커플링을 제거할 수 있었다는 점이 큰 의의이다.

더 이야기를 이어나가기에 앞서 **Concurrent API** 에 추가된 몇 가지 클래스들을 더 이야기해본다. 
## Callable
`Callable` 클래스는 기존에 존재하던 `Runnable` 클래스의 아쉬운 점을 보완하기 위해 고안되었다. `Runnable` 은 그 이름처럼 실행되는 것으로 끝난다. 그러므로 실행 결과가 필요한 경우 공용 메모리나 파이프 같은 것들을 사용해야했다. 이에 반해 `Callable` 은 실행 후 값을 반환한다. 그러므로 반환값의 타입에 대해 제네릭이다.
```java
public interface Callable<V> {
	// @throws Exception : if unable to compute a result
	V call() throws Exception
}
```
## Future
```java
public interface Future<V> {
	// 실행 전이라면 취소, 실행 중이라면 파라미터로 받은 값에 따라서 중단 혹은 계속 진행
	boolean cancel(boolean mayInterruptIfRunning);
	// cancel 이 호출된 뒤에는 무조건 true 반환
	boolean isCancelled();
	// cancel 이 호출된 뒤에는 무조건 true 반환
	boolean isDone();
	// blocking 으로 기다린다. execute 중 exception 이 발생되거나, cancel 되서 exception 발생 가능
	V get() throws InterruptedException, ExecutionException;
	V get(long timeout, TimeUnit unit) throws InterruptedException, ExecutionException, TimeoutException);
}
```
`Executor` 의 개념은 `Callable` 의 비동기적 실행을 의미한다. 그러므로 `Callable` 을 `Executor` 에 제출한 이후의 어떤 시점에서 반환값은 존재할 수도 있고, 아직 채워지지 않았을 수도 있다. `Future` 란, 이러한 시간 의존성을 추상화한 클래스라고 이해했다.
## ExecutorService
`ExecutorService` 인터페이스는 `Executor` 인터페이스를 상속한다. `Executor` 의 라이프 사이클 관리와 `Callable` 타입을 파라미터로 받을 수 있는 인터페이스를 추가로 지닌다. `Future` 를 살펴봤던 이유가 바로 이 `Callable` 타입을 파라미터로 받는 인터페이스를 이해하기 위해서였다.
```java
public interface ExecutorService extends Executor {
...
	<T> Future<T> submit (Callable<T> task);
...
}
```
`Executor` 에서는 `Runnable` 만 파라미터로 받을 수 있었지만, `ExecutorService` 에서는 submit 메소드를 통해 `Callable` 를 파라미터로 받을 수 있다. 이 경우 `Callable` 이 언제 실행될지 모르므로, submit 메소드는 `Callable` 의 제네릭 타입을 바로 반활할 수 없다. 대신 이를 감싼 `Future` 를 반환하는 것이다.