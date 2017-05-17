## Mutex

SICP 에서 `mutex` 에 대해서 살펴봤었다. `mutex` 를 구현하기 위해서는 atomic operation 이 필요했다. 간단하게는 공유되는 자원에 대한 `boolean` 변수를 하나 들고있으면서, `false` 인지 확인 -> `false` 라면 `true` 로 선언하는 두 가지 연산을 atomic 하게 수행하면 된다. 만약 atomic 하지 않다면 동시에 두개 이상의 개체가 `boolean` 변수를 `false` 로 인식하고, 자신이 `mutex` 를 획득했다고 생각할 수 있다. 이러한 `boolean` 변수를 Java 에서는 `AtomicBoolean` 으로 간단하게 구현할 수 있다.

```java
AtomicBoolean atomicBoolean = new AtomicBoolean(false);
...
boolean acquiredMutex = atomicBoolean.compareAndSet(false, true);한다.
``` 

## Semaphore

또한 Java 는 기본 API 에서 `semaphore` 클래스를 제공한다. 사실 `mutex` 는 `semaphore` 의 특수한 경우라고 볼 수 있다. `semaphore` 는 일반적인 N개 짜리 접근 권한을 추상화하므로, `N=1` 인 특수한 경우가 `mutex` 가 되는 셈이다. 또한 `semaphore` 는 대기자들에 대해서, 접근 권한을 요청한 순서대로 줄지 여부도 설정할 수 있다. 

```java
Semaphore semaphore = new Semaphore(10, true); // fair 한 크기 10 semaphore 를 생성
semaphore.acquire(); // 접근 권한을 얻을때까지 스레드가 blocking 되고, thread scheduling 에서도 배제
``` 
 
`AtomicBoolean` / `Semaphore` 클래스 모두 그 기저에는 `sun.misc.Unsafe` 클래스가 받쳐주고있다. 결국 코드를 밑으로 타고, 타고 가다보면 `Unsafe` 클래스의 `native` 메서드들에 부딪힌다. 아마 system call 호출이 필요한 부분은 `native` 메서드로 구현되어있나보다.