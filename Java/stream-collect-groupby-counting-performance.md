[Stream API 의 Collector 를 활용하는 방법](https://github.com/chang12/TIL/blob/master/Java/stream-collect-groupby-counting.md) 글을 쓸때 참 인상적이었다. 코드 자체는 한결 간결하게 작성할 수 있었는데, 성능상으로는 어떨지 궁금해졌다. 아주 간단한 예제를 3가지 방법으로 구현하고 비교해봤다.

테스트에 사용할 데이터는 이전 문서의 `Name` 클래스와 유사한 `Data` 라는 클래스다. 뒤에서 함수 레퍼런스로 사용하기 위해 getter 를 선언했다.

```java
class Data {
    String id;
    String content;

    Data(String id, String content) {
        this.id = id;
        this.content = content;
    }

    String getId() {
        return id;
    }
}
```

### 데이터 준비

간단하게 0 ~ 499 의 id 별로 `Data` 인스턴스를 500개씩 만들어서 크기 25,000 의 List 를 준비한다.

```java
List<Data> dataList = new ArrayList<>();
for (int index = 0; index < 500; index++) {
    String id = String.valueOf(index);
    for (int count = 0; count < 500; count++) {
        dataList.add(new Data(id, UUID.randomUUID().toString()));
    }
}
```

### 테스트

`id` 별로 `Data` 인스턴스의 개수를 카운트하는 간단한 예제 코드를 3가지 방식으로 구현한다.

#### 1. Collection API 만 사용해서 구현

```java
Map<String, Long> countById1 = new HashMap<>();
for (Data data : dataList) {
    countById1.put(data.getId(), countById1.compute(data.getId(), (id, count) -> count == null? 0L : count + 1L));
}
```

#### 2. Stream API 으로 grouping 까지 하고 List 의 크기로 매핑하는건 for loop 으로 구현

```java
Map<String, List<Data>> groupById = dataList.stream().collect(Collectors.groupingBy(Data::getId));
Map<String, Long> countById2 = new HashMap<>();
for (Map.Entry<String, List<Data>> entry : groupById.entrySet()) {
    countById2.put(entry.getKey(), (long) entry.getValue().size());
}
```

#### 3. 모든 과정을 Stream API 로 구현

```java
Map<String, Long> countById3 = dataList.stream().collect(Collectors.groupingBy(Data::getId, Collectors.counting()));
```

### 결과

3번 방법(64ms) < 2번 방법(104ms) < 3번 방법(121ms) 순서로 소요시간이 덜 걸렸다. 일반적인 경우로 단언할 수 없겠지만, 이번에 테스트한 간단한 예제의 경우에는 Stream API 를 더 활용할 수록 더 좋은 성능을 얻을 수 있었다.