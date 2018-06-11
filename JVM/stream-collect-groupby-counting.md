Java 8 Stream API 를 활용해서 아래와 같은 꽤나 복잡한 작업도 한번에 수행할 수 있다는 점이 인상적이어서 기록해둔다.

```java
List<Name> nameList = new ArrayList<>();

// 2종류 성씨에 대한 5명의 이름을 집어넣는다.
nameList.add(new Name("Changhyun", "Lee"));
nameList.add(new Name("Jaemyeong", "Lee"));
nameList.add(new Name("Jaeyong", "Lee"));
nameList.add(new Name("Yebynn", "Cheong"));
nameList.add(new Name("Dongyeong", "Cheong"));

Map<String, Long> countByLastName;
countByLastName = nameList.stream()
                          .collect(Collectors.groupingBy(Name::getLastName, Collectors.counting()));

// 성씨별 개수를 얻을 수 있다.
// {Cheong=2, Lee=3}
System.out.println(countByLastName);
```

`Collectors.counting()` 메서드가 반환하는 Collector 의 generic type 을 이해하는게 어려웠었다. 결과로부터 끼워맞추자면 `Collector<Name, ?, Long>` 을 얻어야 했다. 메서드 코드를 보고 이해할 수 있었다.

```java
// T = Name
public static <T> Collector<T, ?, Long> counting() {
    return reducing(0L, e -> 1L, Long::sum);
}
```

이렇게 보면 좀 어려울 수 있지만, `Collector` 가 사용되는 맥락이 `Collection<Name>`, 상세하게는 `List<Name>` 이라고 머릿속에 가정하고나면 이해할 수 있다. `reducing` 메서드가 받는 파라미터들을 보면 초기값 0에, `Name` 인스턴스 하나는 카운트 1로 매핑되며, 이전까지 누적 카운트값에 이번 카운트 값을 더해서 새로운 누적 카운트 값을 만든다는걸 알 수 있다. 즉, `Collector` 입장에서는 자신을 사용하는 `Collection` 의 크기를 반환하는 셈이다.

`Collectors.groupingBy(Function<Name, String>)` 시그니처의 `Collector` 를 사용하면, `List<Name>` 를 `Map<String, List<Name>>` 으로 grouping 할 수 있음을 생각하자. 여기에 추가적인 이터레이션으로 `Map<String, Long>` 을 간단하게 만들 수도 있겠지만, `Name 의 stream... -> List<Name> -> Long` 의 reduce 과정을 한번에 합칠 경우 더 효율적으로 연산할 수 있게 되므로 이득이다. 