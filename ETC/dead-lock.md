SICP 스터디에서 3.4 절을 공부했는데, 그 내용이 유익해서 정리해본다.

### Mutex

Concurrency 문제를 해결하기 위한 가장 기본적인 방법은 `mutex` 이다. `mutex` 는 **mutual exclusion** 의 약자다. 컴퓨터 프로그램을 이루는 여러 주체가 **공유하는 자원(shared resource)** 이 있을때 이를 문제없이 사용하도록 도와주는 수단이다. 쉽게 얘기해서 공유하는 자원에 **lock** 을 두고, 자원을 사용하려는 주체가 사용하기에 앞서 lock 을 획득하도록 강제히여, 동시에 2개 이상의 주체가 자원에 접근하는것을 막는 개념이다.

이러한 `mutex` 를 구현하기 위해서는 **atomic operation** 이 필수적이다. lock 을 획득할 수 있는지 확인하고, 획득 가능하다면 lock 을 획득하는 연속된 두 연산이 atomic 하게 이뤄짐이 보장되어야 하기 때문이다. `mutex` 의 가장 간단한 구현은 boolean 변수 하나를 사용하는 방법이라고 생각하는데, 변수값이 false 인것을 확인하고, 이를 true 로 assign 하는 과정이 atomic 하게 이뤄져야한다는 뜻이다. atomicity 가 보장되지 않는다면 동시에 2개 이상의 주체가 변수값을 true 로 assign 하면서 자신이 lock 을 획득했다고 생각하고 shared resource 에 접근하게 되므로 문제가 된다.   

이러한 atomic operation 은 OS 레벨 / HW 레벨에서 서포트해줘야 하는 영역이다. 정확하지 않을 수 있는데, 책의 주석을 읽고나서 지금 추측하기로 single processor 머신이라면 kernel 에서 제공할 수 있는 기능인것 같고, multi processor 머신이라면 HW 레벨에서 서포트해줘야 하는 부분인것 같다. 책의 각주에서 MIT Scheme / single processor 머신의 경우를 예로 들어 설명했다.

```scheme
(define (test-and-set! cell)
  (without-interrupts
    (lambda ()
      (if (car cell)
          true
          (begin (set-car! cell true)
                 false)))))
```

### Dead Lock

이러한 `mutex` 가 구현되어있다는 가정하에 concurrency 문제를 해결하기 가장 기본적인 serialization 전략은, 앞서 언급했듯이 shared resource 마다 `mutex` 를 갖게 만들고, 이에 접근하려는 주체는 `mutex` 를 통해 lock 을 획득하도록 규칙을 정하는 것이다.

하지만 문제가 존재한다. shared resource 가 하나인 경우에는 문제가 없지만, 두개 이상인 경우에 어느 주체가 resource 1 -> resource 2 순서로 접근하고, 다른 주체가 resource 2 -> resource 1 순서로 접근하는 경우를 생각해보면 첫번째 주체가 작업을 완료하기 위해서 resource 1 의 lock 을 획득하고, 두번째 주체가 작업을 완료하기 위해서 resource 2 의 lock 을 획득한 경우에 완전한 작업의 완료를 위해서 서로의 lock 이 필요하지만, 정작 그 필요한 lock 이 없어서 둘 다 작업을 완료하지 못하는 봉착 상태에 빠진다. 이를 **dead lock** 이라고 부른다.

![dead lock](https://pics.onsizzle.com/ashwani-gautam-yesterday-at-08-14-i-explain-us-deadlock-and-17211984.png)

이러한 dead lock 을 해결하는 첫번째 방법은, shared resource 마다 **크기 비교가 가능한 unique identifier** 를 매기는 방법이다. 주체는 실행 완료를 위해 필요한 모든 shared resource 를 파악해서 unique identifier 가 단조증가하는 순서로 차례대로 lock 획득을 시도한다. 이 방법으로 dead lock 을 해결할 수 있으나 실질적으로 주체가 필요한 모든 shared resource 를 실행 시작 이전에 파악한다는 것이 불가능한 경우가 많다고 한다. 그러므로 dead lock 발생을 미연에 100% 방지하는 것은 사실상 불가능하며, dead lock 상태에 빠졌을때 이를 헤어나오기 위한 dead lock recovery mechanism 이 주로 쓰이는 방법이라고 한다.
