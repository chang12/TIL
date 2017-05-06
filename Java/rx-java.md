> **Reactive X** 는 Observer 패턴과 Iterator 패턴, 그리고 함수형 프로그래밍의 아이디어를 조합한 형태이다. **RxJava** 는 이런 Reactive X 를 Java 에서 사용할 수 있도록 개발된 라이브러리이다.

## Download

[GitHub repository](https://github.com/ReactiveX/RxJava) 의 README.md 파일을 참고해서 Maven 으로 RxJava2 를 다운로드했다.

## Flowable vs Observable

Reactive X 시리즈에서 핵심 개념은 `Observable` 이라고 알고 있었다. 그런데 README 에 있는 간단한 소개를 읽다보니 `Observable` 보다 `Flowable` 이 더 핵심 객체로 다가왔다. 둘 다 0 ~ N 개의 임의의 데이터를 내뱉는 asynchronous stream 를 추상화하는 객체라는 점은 동일하나, `Flowable` 은 Reactive-Streams  와 backpressure 를 추가로 지원한다고 적혀있다. 그리고 대충 훑어본 느낌으로 RxJava2 는 RxJava1 에서의 교훈을 가지고 Reactive-Streams 위에 새롭게 설계한 프로젝트였기에 Reactive-Streams 를 지원하는 `Flowable` 이 더 최신 객체로 생각되었다.

이에 대해서 GitHub Wiki 의 [What's different in 2.0](https://github.com/ReactiveX/RxJava/wiki/What's-different-in-2.0#observable-and-flowable) 문서에 소개하는 항목이 있었다. 그러나 RX 경험이 전무한 상태다보니 와닿지가 않았다. 그러므로 차분히 backpressure 에 대해 먼저 살펴봤다.

## Backpressure

마찬가지로 GitHub Wiki 의 [Backpressure](https://github.com/ReactiveX/RxJava/wiki/Backpressure) 항목이 있었다. 우선 backpressure 문제가 논의되는 맥락은, Observable 이 item 을 emit 하는 속도를 Observer 가 따라가지 못하는 상황이다. 이 경우 Observable 이 emit 하고, Observer 가 처리하기 이전의 상태의 item 들이 마구 버퍼에 쌓이게 되고, 궁극적으로 out of memory 에러가 발생할 수 있다. 간단한 예로는 naive 한 zip operation 구현에서 zip 의 대상이 되는 두 Observable 의 emit 속도가 현저히 차이나는 경우를 들 수 있겠다.

이때 backpressure API 는 주도권을 Observable 에서 Observer 로 옮긴다. 즉, Observer 가 emit 한 item 을 Observer 가 process 하는 기존의 행테에서, Observer 가 자신이 처리할만큼의 item 만 Observable 에 request 하는 pulling 의 행태로 변환시키는 것이다. 이를 **reactive pull backpressure** 라고 부른다. 만약 Observable 구현체가 이러한 backpressure 를 지원한다면 (request 메서드 호출에 대해 온디맨드로 item 을 emit 한다면) 잘 동작할 것이다. 하지만 이는 Observable 이 필수로 따라야 할 규약이 아니기 때문에, 만약 Observable 이 이에 개의치않고 신나게 item 을 emit 한다면 다른 해결 방법이 필요해진다. 

첫번째 접근법은 `operator` 를 활용해서 Observable 의 emission 을 throttle 하는 방법이다. 특정 duration 중 가장 마지막 item 만 받아오기 / emission 후 일정 duration 동안 추가적인 emission 이 없는 경우에만 item 받아오기 등의 기법이 사용될 수 있다. RxJava 2 에서 이러한 operator 들을 제공한다.

두번째 접근법은 backpressure 과 유사한 기능을 제공하는 `operator` 를 사용하는 방법이다. 적다보니 첫번째/두번째 접근법이 유사해보이지만 문서에서 분리된 문단이므로 분리해서 생각하려고 시도해보자. `onBackPressureBuffer` 등이 그 예이다.

## subscribeOn / observeOn

GitHub README 문서를 따라가다보면 간단한 예제 코드를 만날 수 있는데 Fluent 하게 method chain 을 만드는 과정에서 `subscribeOn` 도 있었고, `observeOn` 도 있었다. 둘 다 `Scheduler` 를 파라미터로 받는다. 

`Scheduler` 는 RxJava 에서 Thread / Executor 계층을 한단계 더 감싼 추상화 객체이다. 즉, 구체적으로 어떻게 thread pool 을 관리하는지는 `Scheduler` 계층 아래로 숨겨두고 개발자는 `Scheduler` 를 통해 더 간편하게 프로그래밍 할 수 있게 돕는다. Java concurren API 에서 `Executor` / `Executors` 관계와 유사하게 `Schedulers` 라는 유틸리티 클래스에서 다양한 `Scheduler` 팩터리 메서드를 제공한다.

Thread 이름을 콘솔에 찍는 식으로 확인해보니 `subscribeOn` 은 asynchronous task 를 실행하는 `Scheduler` 를 선언할때 쓰이고, `observeOn` 은 task 가 emit 하는 item 을 처리하는 task 가 실행되는 `Scheduler` 를 선언할때 쓰인다. `subscribeOn` 에 넘길 파라미터로 `Schedulers.io()` 팩터리 메서드의 반환값을 사용하는데, 보면 `Schedulers.computation()` 팩터리 메서드도 존재한다. asynchronous task 가 I/O bound 할때와 computational work 일때 다른 `Scheduler` 를 사용한다는게 흥미로웠다.
+
