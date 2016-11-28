`CompletableFuture` 에 대해 알아보려고 API 문서에 들어갔다가, `CompletableFuture` 가 `Consumer` 인터페이스를 구현한다고 해서 궁금해졌었다.

`Consumer<T>` 는 JDK 1.8 부터 등장한 인터페이스다. 이름에서 알 수 있듯이 `T` 타입의 파라미터를 받아서 어떤 동작을 수행하는 **accept** 메소드를 지니고, 그게 유일한 abstract method 이다. accept 메소드 외에는 **andThen** 이라는 메소드를 지니는데 default 로 구현이 되어있다.
```java
default Consumer<T> andThen(Consumer<? super T> after {
	Objects.requireNonNull(after);
	return (T t) -> { accept(t); after.accept(t); };
}
```
또한 Functional Interface 이므로 JDK 1.8 부터 지원하는 **Lambda** 로 구현할 수 있다. 