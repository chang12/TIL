## IntelliJ Project
* Project 를 생성할때 Maven 을 사용하는 쪽으로 설정
* 만들어진 pom.xml 에 Guava 가이드 문서에서 소개한 dependency section 을 추가
```xml
<dependency>
  <groupId>com.google.guava</groupId>
  <artifactId>guava</artifactId>
  <version>19.0</version>
</dependency>
```
* 완료! Guava 를 사용할 수 있다.

## Guava Cache
[가이드 링크](https://github.com/google/guava/wiki/CachesExplained)
Cache 와 ConcurrentMap 을 주로 비교하고 있다. 가장 큰 차이는 ConcurrentMap 에 저장된 Entry 는 JVM 이 꺼지기 전까지 영원하지만, Cache 는 그러하지 않다는 점이다.

CacheLoader 로 구현하는 법과, Callable 으로 구현하는 법이 있다.

Eviction 에 대해서 얘기한다. Cache 를 쓴다는 것 자체가, 애초에 다루는 문제에서 저장해둬야 할 데이터의 양이 메모리 크기 이상이라는 뜻이다. 그러므로 어떤 정책에 따라서 "축출" 해야한다.
* size-based: 크기 제한을 넘기 직전부터 축출한다. 단순 Entry 개수가 아니라, weight sum 으로 제한할 수도 있다. 그러려면 weigh 함수를 오버라이딩해서 Weigher 인터페이스를 구현하면 된다. 
* time-based: 읽기/쓰기 직후 얼마 이상의 시간이 흐르면 해당 Entry 를 축출한다.
* reference-based: 무슨 얘기인지 이해 못했다. weak/soft/strong reference 에 대해서 이해해야 할 것이다.

RemovalNotification 을 인자로 받는 onRemoval 메소드를 오버라이딩 하면서 RemovalListener 인터페이스를 구현하고, Cache 에 등록할 수 있다. 기본값으로 RemovalListener 의 동작은 synchronous 하므로, 너무 비싼 연산이면 Cache 의 성능이 저하된다. asynchronous 하게 만드려면 RemovalListeners.asynchronous annotation 을 쓸 수 있다.