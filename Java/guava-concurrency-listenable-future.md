(회사에서 9월 30일에 그 필요성을 느꼈었다.)

https://github.com/google/guava/wiki/ListenableFutureExplained

## ListenableFuture
`ListenableFuture` 는 JDK 의 `Future` 인터페이스를 확장한 것이다. 그러나 Guava 에서는 `Future` 대신 `ListenableFuture` 를 사용하라고 강력히 권고한다. 이러한 `ListenableFuture` 인터페이스는 하나의 메소드를 지닌다. 
```java
public interface ListenableFuture<V> extends Future<V> {
	void addListener(Runnable listener, Executor executor);
}
```
`ListenableFuture` 에 표현된 연산이 종료됬을 때 걸고 싶은 **Callback** 이 있다면, 그 실행을 `Runnable` 과 `Executor` 의 관계로 기술할 수 있다. Concurrent API 의 기본철학과 동일하게, 걸고 싶은 Callback 도 바로 실행되는 것이 아니라 `Executor` 에게 맡겨질 것이다.
## ListenableExecutorService
JDK 의 `ExecutorService` 의 submit 메소드에서 `Future` 를 반환했었다. 그러므로 Guava 에도 `ListenableFuture` 를 반환하는 무언가가 있어야 할 것이다. `ListenableExecutorService` 가 그것이다. `ExecutorService` 를 Listenable 하게 변환하는 메소드를 `MoreExecutors` 클래스가 정적으로 제공한다.
```java
public final class MoreExecutors {
	...
	public static ListeningExecutorService listeningDecorator(ExecutorService delegate) {
		...
	}
	...
}
```
## Callback 등록과 FutureCallback
`ListenableFuture` 에 값이 채워지고 난 이후에 Callback 을 등록하는 이슈에 대해 얘기해보자.

Callback 이 `ListenableFuture` 값과 독립적인 실행이라면, `ListenableFuture` 의 addListener 메소드를 사용할 수 있다. 앞서 살펴봤던 내용이다.
```java
publi interface ListenableFuture<V> extends Future<V> {
	void addListener(Runnable listener, Executor executor);
}
```
독립적이지 않고 값에 의존하는 실행이라면 Guava 가 제공하는 `FutureCallback` 인터페이스를 활용할 수 있다. 반환된 값 혹은 중간에 던져진 `Exception` 이 `FutureCallback` 메소드의 파라미터로 들어갈 것이다. 
```java
public interface FutureCallback<V> {
	void onSuccess(@Nullable V result);
	void onFailure(Throwable t);
}
```
`ListenableFuture` 와 `FutureCallback` 을 연계할때는 `Futures` 클래스의 정적 메소드들을 사용한다.
```java
public static <V> void addCallback(ListenableFuture<V> future, FutureCallback<? super V> callback) {
	addCallback(future, callback, directExecutor());
}
public static <V> void addCallback(final ListenableFuture<V> future, final FutureCallback<? super V> callback, Executor executor) {
	// 코드는 생략
}
```
addListener 와 마찬가지로 `Executor` 를 지정할 수 있고, 지정하지 않은 경우에는 동일한 Thread 에서 바로 실행된다. 오버로딩된 두 메소드 모두 반환형이 void 이므로 이후 추가적인 chaining 은 불가능하다.
## Work Flow
1. `ExecutorService` 인스턴스(=executor) 생성
2. `Callable` 인스턴스(=callable) 생성
3. executor 에 callable 을 submit 해서 `ListenableFuture`(=future) 획득
4. Callback 등록
	- `Runnable` 인스턴스 생성하고, future 의 addListener 메소드로 등록
	- `FutureCallback` 인스턴스 생성하고, `Futures` 의 addCallback 에 future 와 함께 등록

## AsyncFunction
### Definition
입력 타입의 값을 받아서, 출력 타입 제네릭의 `ListenableFuture` 를 반환하는 작업을 기술한 인터페이스다. 해당 작업의 메소드 하나를 지닌다.
```java
public interface AsyncFunction<I, O> {
	ListenableFturue<O> apply(@Nullable I input) throws Exception;
}
```   
### Example Usage
`Futures` 클래스의 transformAsync 메소드 등에서 활용된다. `ListenableFuture` 인스턴스와 함께 메소드의 파라미터로 들어가서 `ListenableFuture` 의 제네릭 타입을 변환하는 역할을 맡는다.
```java
public static <I, O> ListenableFuture<O> transformAsync(
	ListenableFuture<I> input, 
	AsyncFunction<? super I, ? extends O> function
) {...}
public static <I, O> ListenableFuture<O> transformAsync(
	ListenableFuture<I> input, 
	AsyncFunction<? super I, ? extends O> function, 
	Executor executor
) {...}
```
input 에 값이 채워진 이후에는 `AsyncFunction` 의 apply 메소드가 쓰일 것이고, cancel 되거나 `Exception` 을 던질 경우에는 output 으로 propagate 될 것이다. 
## Application
`ListenableFuture` 의 가장 큰 의의는 **asynchronous operations** 로 이뤄진 복잡한 chain 을 구성하는게 용이하다는 점이다. 

### Transform Type
앞서 언급한 `Futures` 의 transformAsync 메소드가 `ListenableFuture` 의 제네릭 타입을 변환해줬었다.
### Fan-Out
같은 `ListenableFuture` 인스턴스를 가지고, 위의 transformAsync 메소드를 여러번 호출해도 문제가 없다. 
### Fan-In
`Futures` 의 allAsList 메소드 등을 활용할 수 있다. 앞단이 `ListenableFuture` 들의 값이 모두 넣어져야만 뒷단의 `ListenableFuture` 도 값이 넣어진다. 하나라도 cancel 되거나 `Exception` 이 발생하면 결과로 propagate 한다.