SICP 스터디에서 Set 인터페이스 내용을 다뤘다. 책에서 Set 인터페이스를 unordered list / ordered list / binary tree 의 세가지 방식(?)으로 구현했다. Java 에서는 어떤 구현체들이 있을지 문득 궁금해졌다.

## Set

* SICP 책의 `union-set` 메서드에 대응하는건 Set 인터페이스의 `addAll` 메서드이다.
* SICP 책의 `intersection-set` 메서드에 대응하는건 Set 인터페이스의 `retainAll` 메서드이다. 메서드를 호출하면 intersection 이 아닌 원소들이 제거되므로, `new HashSet(s)` 생성자로 deep copy 하고 호출하는게 좋을 것이다.

## HashSet / LinkedHashSet

### HashSet

* back-end 에 `HashMap` 인스턴스가 사용되는 Set 구현체다.
* add / contains / remove / size 같은 basic operation 들의 **O(1) time complexity** 를 보장한다.
* Set 에 대한 iteration 에 대해서는 순서가 유지되지 않는다.

### LinkedHashSet

* 내부 entry 들을 doubly-linked list 로 관리해서 iterator 의 순서를 Set 에 추가된 순서로 유지해준다.

## SortedSet / NavigableSet / TreeSet

* `SortedSet` 은, `Comparator` 인터페이스를 구현하는 클래스의 인스턴스들에 대해서, 크기 순서 나열을 보장해주는 Set 이다. Set 인터페이스를 확장한다.
* `NavigableSet` 은 어떤 인스턴스와 가장 인접한 Set 의 entry 를 반환하는 인터페이스를 추가로 제공한다. `SortedSet` 을 확장한다.
* `TreeSet` 은 `NavigableSet` 을 구현한다. `HashMap` 이 `HashSet` 을 뒷받쳐주듯이, `TreeMap` 이 뒷받쳐준다. basic operation 들의 **O(log n) time complexity** 를 보장한다.

java.util 패키지 뿐 아니라, java.util.concurrent 패키지에서도 다양한 구현체들이 더 존재한다. 실제 상황에 맞닿뜨렸을때 직접 찾아보는게 더 값진 공부가 될 것 같아 굳이 이번에 찾아서 정리하지는 않는다.