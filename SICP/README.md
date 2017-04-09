## 2017-04-01 (Sat)

* 존재하지 않는 symbol 에 대해 `set!` procedure 를 호출하면 에러가 발생한다. 그래서 초기값을 `define` 구문으로 정의하고 그에 대해 `set!` procedure 를 호출해야되서 불편했다. 3.2 절을 읽으니 왜 그랬는지 이해가 된다. `set!` procedure 를 호출하면, symbol 을 찾아서 frame sequence 를 순회하고, 처음으로 발견한 binding 의 value 를 교체하는 식으로 동작하기 때문이다.
* recursive process 는 stack machine 과, iterative process 는 register machine 과 연관됬었다는 점을 **Chapter 1.2.1** 를 다시 읽으며 깨우쳤다. returned value 가 call to call 로 전달되는 개념에 대해서 Chapter 5 를 예고했었는데, 여기서도 Chapter 5 를 예고한다. **recursive procedure** 면서 **iterative process** 인게 가능하다는걸 기억하자. 예를 들어 `factorial` 을 `factorial-iter` 로 구현하면 `factorial-iter` 이 자기 자신을 호출하므로 recursive procedure 지만, 또한  iterative process 이다. recursion 연산이 펼쳐지는게 아니라 매번 state 를 갱신하면서 자기 자신을 호출하기 때문이다. 하지만 modern 프로그래밍 언어들은 recursion 으로 프로그래밍하면 새로운 procedure 호출이 있을때마다 새로운 stack 을 만든다. 그러므로 필요한 메모리 공간의 크기가 recursion 깊이에 비례하게되어 iterative process 로 recursion 을 구현할 수 없다. 따라서 iterative process 를 위한 특별한 구문을 제공하는데 그게 바로 `for`, `while` 등의 반복문이다. 이와 달리 Scheme 은 언어 차원에서 iterative process 를 제공하는데 이를 `tail recursion` 이라고 부른다.
* 졸립지만 자기전에 신속하게 3.2.4 절을 훑으며 읽어봤다. procedure call = new environment creation 이라고 인식하게 됬다. 지금 들여다보고있는 이 내용이 `closure` 인건가 싶다. 만약 맞다면, 앞으로 누군가 `closure` 의 개념에 대해 묻는다면 SICP 의 Chapter 3.2 를 추천해주고 싶다.

## 2017-04-09 (Sun)

* `set-car!` 과 `set-cdr!` 를 사용하기 위한 첫 시도는 근우가 알려준 neil/sicp 를 사용하는 방법이었다. `#lang planet neil/sicp` 로 언어 설정을 변경하면 사용할 수 있었다. 문제는 이렇게 언어 설정을 변경하고나면 `require` / `provide` 구문을 사용할 수 없었다.
* 두번째 방법은 스터디 시간에 근우가 알려준 `require sicp/mpair` 로 패키지를 임포트하는 방법이었다. 이 경우 언어 설정은 racket 으로 그대로 남기 때문에 `require` / `provide` 를 그대로 사용할 수 있다. `cons`, `car`, `cdr` 대신 `mcons`, `mcar`, `mcdr` 을 사용해야하는게 좀 번거로웠다.
* 집에와서 찾아보니, `#lang planet neil/sicp` 는 옛날 DrRacket 을 위한 방법이고, 그 최신버전은 깔끔하게 사용할 수 있는 [SICP Collection](http://docs.racket-lang.org/sicp-manual/index.html) 이었다. `require sicp` 인걸보니 `require sicp/mpair` 는 패키지의 일부만 임포트하는 선언이었나보다. `require sicp` 로 임포트하면 `cons`, `car`, `cdr` 을 그대로 사용할 수 있으니 가장 쾌적하다.