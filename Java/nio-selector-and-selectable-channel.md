`Selector` 와 `SelectableChannel` 이 어떻게 함께 쓰이는지 궁금했다.

### Selector 에 SelectableChannel 을 등록

```java
Selector selector = Selector.open(); // 기본 SelectorProvider 를 사용한다 -> 커스텀 정의도 가능
SelectableChannel sc = ...
sc.register(selector, SelectionKey.OP_READ); // attachment 는 뭔지 모르는 상태
```

`register` 메서드에 파라미터로 관심가지는 operation 에 대한 정수형 식별자(interested ops 라고 부름) 를 넘기면서 호출하면, `SelectionKey` 를 반환한다. 지금까지 본 예제에서는 반환된 `SelectionKey` 인스턴스를 직접 사용하지는 않았다.

### Selector 에 등록된 SelectableChannel 중 연산이 준비된 객체들에 접근

```java
Selector selector = ...
...
selector.select(); // 먼저 select 메서드를 호출 -> 의미는 모르는 상태
Iterator<SelectionKey> iter = selector.selectedKeys().iterator(); // 보통 Iterator 로 사용
```
