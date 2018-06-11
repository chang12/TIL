`AtomicLongMap` 에 대해서는 예전부터 궁금했었지만 대충 그러려니하고 넘어갔었다. 간단히 한번 짚고 넘어가보려고 한다. `AtomicLongMap` 의 효과를 느끼기 위해서 비교대상으로 `HashMap` 을 만들어서 테스트해보려고 한다. 멀티 스레드 환경에서 두 가지 `Map` 에 접근해서 신나게 값을 증가시키고 그 결과를 비교할 계획이다.   

### HashMap

```java
// Map 초기화
Map<String, Long> hashMap = new HashMap<>();
hashMap.put("count", 0L);

// 큰 횟수만큼 Map 의 Entry 값을 변화시키는 Runnable 준비
Runnable runnable = () -> {
    for (int iter = 0; iter < 1000000; iter++) {
        int currCount = pureMap.get("count");
        pureMap.put("count", currCount + 1);
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

### AtomicLongMap

```java
// 초기화도 필요없다. implicitly 0 으로 추정한다.
AtomicLongMap<String> atomicLongMap = AtomicLongMap.create();

// 단순히 put/get 이 아니라 다양한 메서드를 제공한다.
Runnable runnable = () -> {
    for (int iter = 0; iter < 1000000; iter++) {
        atomicLongMap.incrementAndGet("count");
    }
};

Thread t3 = new Thread(runnable);
Thread t4 = new Thread(runnable);
t3.start();
t4.start();
t3.join();
t4.join();

System.out.println(atomicLongMap.get("count"));
``` 
```
2000000
```

### 결과

`AtomicLongMap` 의 경우 정상적인 결과를 얻었고, `HashMap` 의 경우 결과값이 **2,000,000** 이 아니라 **1,146,530** 으로 나왔다. 이를 통해 `HashMap` 은 Entry 들이 Thread-safe 하지 않다는 사실을 확인할 수 있다. 그러므로 멀티 스레드 환경에서 값의 지속적인 increment/decrement 가 필요하고, 업데이트되는 값을 담아놓을 컨테이너가 필요하다면, `AtomicLongMap` 이 적절할 것이다.
