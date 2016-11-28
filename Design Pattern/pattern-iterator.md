## introduction
iterator pattern 을 사용할 경우 **algorithm** 과 **container** 를 분리할 수 있는 장점을 얻는다. 예를 들어 Java 에서 배열의 특정 조건을 만족하는 서브배열을 획득하는 알고리즘을 구현하는 상황을 생각해보자. 

만약 algorithm 과 container 가 분리되지 않은 상태에서 구현하려고 시도한다면, 알고리즘 구현을 위해 설계하는 클래스에 불필요한 요소들이 들어간다. 알고리즘을 적용할 배열을 파라미터로 받는 생성자, 배열을 담고 있는 필드, 배열의 조작을 위한 인터페이스 등이 모두 고려되야 하기 때문이다. 이는 알고리즘 자체에 집중하기 힘들게 만든다.

하지만 **iterator pattern** 을 적용한 **container** 가 독립적으로 존재한다면, 알고리즘을 기술하는 클래스에서는 메서드의 파라미터로 이 container 의 인스턴스를 받는것으로 충분하다. 이런 맥락에서 algorithm 과 container 의 분리가 중요하다.
## iterator pattern in Java
Java 언어에서는 iterator pattern 을 편리하게 사용할 수 있도록 **Iterator** 인터페이스를 제공한다. 메서드들의 이름과 시그니처를 보면 그 내용을 쉽게 유추할 수 있다.
```java
public interface Iterator<E> {
	boolean hasNext();
	E next();
	
	default void remove() {...}
	default void forEachRemaining(Consumer<? super E> action) {...}
}
```