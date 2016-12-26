예전부터 궁금했었으나, 대충 그러려니하고 넘어갔던 자료형이 `AtomicLongMap` 이다. 간단히 한번 짚고 넘어가자.

`AtomicLongMap` 의 효과를 느끼기 위해서, 비교대상으로 vanilla `HashMap` 을 만들어서 테스트해본다. 결과값이 **2,000,000** 이 아니라 **1,146,530** 으로 나옴에 주목하자. 즉, vanilla `HashMap` 은 Entry 들이 Thread-safe 하지 않다.   

```java
// Map 초기화
Map<String, Long> pureMap = new HashMap<>();
pureMap.put("count", 0L);
// 큰 횟수만큼 Map 의 Entry 값을 변화시키는 Runnable 준비
Runnable runnable = ()->{
    for (int iter = 0; iter < 1000000; iter++) {
        pureMap.put("count", pureMap.get("count") + 1);
    }
};
// Thread 2개가 동시에 Map 을 manipulate 하도록 실행
Thread t1 = new Thread(runnable);
Thread t2 = new Thread(runnable);
t1.start();
t2.start();
t1.join();
t2.join();

// 결과 확인
System.out.println(pureMap.get("count"));
```
```shell
1146530
```

같은 실험을 AtomicLongMap 에 대해서 진행한다. 정확히 기대하던 값이 나오는걸 확인할 수 있다.

```java
// 초기화도 필요없다. implicitly 0 으로 추정한다.
AtomicLongMap<String> atomicLongMap = AtomicLongMap.create();

// 단순히 put/get 이 아니라 다양한 메서드를 제공한다.
Runnable runnable2 = ()->{
    for (int iter = 0; iter < 1000000; iter++) {
        atomicLongMap.incrementAndGet("count");
    }
};

Thread t3 = new Thread(runnable2);
Thread t4 = new Thread(runnable2);
t3.start();
t4.start();
t3.join();
t4.join();

System.out.println(atomicLongMap.get("count"));
``` 
```
2000000
```

여러 thread 가 병렬적으로 수행하는 작업에 대해서, 결과에 대한 간단한 통계치를 저장하기 위한 용도로 활용할 수 있을 것이다. 통계치의 이름을 Entry 의 Key 로 가져가고, **thread-safe** 하게 increment/decrement 할 수 있으므로.