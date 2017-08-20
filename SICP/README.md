## 2017-08-20 (Sun)

* 규진님 마지막 (ㅠㅠ)
* Exercise 4.5 완료 (~ PM 06:14). `application?` 에서 처리해주도록 `make-if` procedure 를 호출할때 파라미터 구성하는 과정에서 `cons` 가 아니라 `list` 를 사용해야했었음.
* Exercise 4.6 완료 (~ PM 07:06). 흥미롭게도 Exercise 4.5 와 반대였는데, `application?` 에서 처리해주도록 `make-lambda` 로 만든 lambda object 와 expressions 들을 넘길때 `list` 대신 `cons` 로 넘겨야했음. `list` 로 넘기니까 `1` 을 procedure 로 인식해버림.

전반적으로 `cons` 와 `list` 에 대한 이슈가 있었는데...

```racket
(define b (list 2 3 4))

(cons 1 b) --> 길이 4짜리
(list 1 b) --> 길이 2짜리
```

전자는 길이 4 / 후자는 길이 2 가 되는 차이점 때문이다. Exercise 4.5 에서는 후자였어야 하는데 전자로 했었어서 `cadr` 이 parameter 를 3개 받았다고 에러가 발생한 것이었다.

## 2017-07-16 (Sun)

* `interpreter.rkt` 파일 하나로 합치기
* `(+ a 10)` 수준의 간단한 evaluation 구현

## 2017-05-22 (Sun)

* `assignment` 는 **"time variation in real world"** 를 **"time variation in the computer"** 로 표현하는 한 가지 방법이었다. 다른 방법도 있다. 바로 `stream` 이라는 자료구조를 사용하는 방법이다. 어짜피 컴퓨터가 표현하는 time 이라는건 discrete step 으로 구현된다. `stream` 은 이를 사용해서 time function 을 infinite sequence 로 모델링한다. `stream` 이 단순히 `list` 로 표현된 sequence 에서 진화하는 과정에서 delayed evaluation 이 등장한다.
* `delay` 의 반환형은 `promise` 이다. 지금은 값을 evaluate 하지 않았지만, 나중에 원하는 시점에 evaluation 해주겠다는 약속이라는 측면에서 이해하면 정말 직관적인 이름이다. `force` 로 값을 evaluate 한다. 
* `call-by-name` / `call-by-need` 라는 개념이 소개되는데, 정확히 이해하지 못했다.
* `cons-stream` 과 `delay` 는 `define` 구문을 사용해서 procedure 로 정의하는 것이 불가능하다. Scheme 과 마찬가지로 Racket 도 **applicative-order evaluation** 으로 동작한다. 그러므로 **"evaluate the arguments and then apply"** 정책으로 동작한다. 그러므로 `(define (cons-stream a b) (cons a (delay b))` 으로 `cons-stream` 을 `define` 구문으로 정의하면 `delay` 에 `b` 를 넘기기 전에 미리 evaluation 하므로 결국 lazy-evaluation 하지 못한다. 책에서도 56번 주석에서 `cons-stream` 과 `delay` 는 **special form** 으로 정의해야한다고 적혀있다. 이 문제에 대해 지난주 스터디때 스터디원들이 많은 고민이 있었고, `define` 대신 `define-syntax` 구문으로 정의해야한다는걸 파악했다. 실제로 `sicp` 패키지에서도 `define-syntax` 로 구현되어있다. 아마 책에서는 4.2절에서 lazy evaluation 에 대해 다루는 내용이 있는데, 여기서 소개되지 않을까 싶다. `stream-filter` 도 마찬가지다.
* implicit `stream` 을 이해하는게 처음에 어려웠다. 하지만 infinite `stream` 을 정의할때 정말 편리하다. 점화식을 기술한다고 생각하고, evaluation 되는 순서가 점화식의 index 라고 생각하면 그래도 쉽게 문제를 해결할 수 있다.
* procedure 가 implicit 하게 정의된 `stream` 을 반환하고 싶을때 내부에서 `define` 해줘야 한. 이때 `let` 을 사용해서 local variable 로 정의하려고하면 unbound identifier 에러가 발생한다. 정의에 자기 자신에 대한 참조가 필요하기 때문이다. 그러므로 `define` 구문은 다르게 동작한다고 추측할 수 있다.  

## 2017-04-30 (Sun)

* **serialized** 와 비교해서 **interleaved** 의 개념을 이해한것이 한가지 수확이다. 
* concurrency 문제는 데이터베이스 수업 시간에나 꽤 심도있게 다뤘었다. 그러나 이는 비단 데이터베이스의 문제가 아니라 CS 의 각 계층별로 존재하는 문제라는 점을 다시 상기했다.
* `mutex` 구현과 관련된 45번 주석을 읽어보다가 의문이 생겨서 [Busy Waiting 위키피디아](https://en.wikipedia.org/wiki/Busy_waiting#Alternatives) 를 읽었는데 유익했다. `mutex` 획득을 기다리는 개체(process 라고 쓰려다가, multi-thread 가 생각나서....) 한테는 아예 time slice 를 나눠주지 않는게 효율적일텐데 이를 지원하는 system call 이 있다는 얘기였다. 그러므로 Java 같은 언어에서도 `synchronized` 구문을 구현할때 이러한 system call 을 사용했을 것 같다.
* mutex 획득을 위해서는 atomic operation 이 필요하다는 내용과, atomic operation 의 예시를 든 46번 주석을 읽었는데 새롭게 다가왔다. 어플리케이션 코드에서 OS 에게 나를 interrupt 하지 말라고 요청(명령) 할 수 있다는 얘기인데, 처음 접하는 개념이었다.
* dead lock 의 개념으로 자연스럽게 전환된다. 가장 간단한 방법은 shared resource 에 unique identifier 를 매기고, 늘 낮은 identifier 값의 resource 의 mutex 부터 획득하도록 규칙을 정하는거라고 하는데 그럴싸했다. 그러나 늘 가능한 방법은 아닌가보다. 일반적으로는 transaction processing 이라는 이름의 주제로 다뤄지는 듯.
* **barrier synchronization** 이라는 개념도 새롭고, 이를 위한 **instruction level** 의 서포트가 있다는 것도 놀랍다.

## 2017-04-09 (Sun)

* `set-car!` 과 `set-cdr!` 를 사용하기 위한 첫 시도는 근우가 알려준 neil/sicp 를 사용하는 방법이었다. `#lang planet neil/sicp` 로 언어 설정을 변경하면 사용할 수 있었다. 문제는 이렇게 언어 설정을 변경하고나면 `require` / `provide` 구문을 사용할 수 없었다.
* 두번째 방법은 스터디 시간에 근우가 알려준 `require sicp/mpair` 로 패키지를 임포트하는 방법이었다. 이 경우 언어 설정은 racket 으로 그대로 남기 때문에 `require` / `provide` 를 그대로 사용할 수 있다. `cons`, `car`, `cdr` 대신 `mcons`, `mcar`, `mcdr` 을 사용해야하는게 좀 번거로웠다.
* 집에와서 찾아보니, `#lang planet neil/sicp` 는 옛날 DrRacket 을 위한 방법이고, 그 최신버전은 깔끔하게 사용할 수 있는 [SICP Collection](http://docs.racket-lang.org/sicp-manual/index.html) 이었다. `require sicp` 인걸보니 `require sicp/mpair` 는 패키지의 일부만 임포트하는 선언이었나보다. `require sicp` 로 임포트하면 `cons`, `car`, `cdr` 을 그대로 사용할 수 있으니 가장 쾌적하다.

## 2017-04-01 (Sat)

* 존재하지 않는 symbol 에 대해 `set!` procedure 를 호출하면 에러가 발생한다. 그래서 초기값을 `define` 구문으로 정의하고 그에 대해 `set!` procedure 를 호출해야되서 불편했다. 3.2 절을 읽으니 왜 그랬는지 이해가 된다. `set!` procedure 를 호출하면, symbol 을 찾아서 frame sequence 를 순회하고, 처음으로 발견한 binding 의 value 를 교체하는 식으로 동작하기 때문이다.
* recursive process 는 stack machine 과, iterative process 는 register machine 과 연관됬었다는 점을 **Chapter 1.2.1** 를 다시 읽으며 깨우쳤다. returned value 가 call to call 로 전달되는 개념에 대해서 Chapter 5 를 예고했었는데, 여기서도 Chapter 5 를 예고한다. **recursive procedure** 면서 **iterative process** 인게 가능하다는걸 기억하자. 예를 들어 `factorial` 을 `factorial-iter` 로 구현하면 `factorial-iter` 이 자기 자신을 호출하므로 recursive procedure 지만, 또한  iterative process 이다. recursion 연산이 펼쳐지는게 아니라 매번 state 를 갱신하면서 자기 자신을 호출하기 때문이다. 하지만 modern 프로그래밍 언어들은 recursion 으로 프로그래밍하면 새로운 procedure 호출이 있을때마다 새로운 stack 을 만든다. 그러므로 필요한 메모리 공간의 크기가 recursion 깊이에 비례하게되어 iterative process 로 recursion 을 구현할 수 없다. 따라서 iterative process 를 위한 특별한 구문을 제공하는데 그게 바로 `for`, `while` 등의 반복문이다. 이와 달리 Scheme 은 언어 차원에서 iterative process 를 제공하는데 이를 `tail recursion` 이라고 부른다.
* 졸립지만 자기전에 신속하게 3.2.4 절을 훑으며 읽어봤다. procedure call = new environment creation 이라고 인식하게 됬다. 지금 들여다보고있는 이 내용이 `closure` 인건가 싶다. 만약 맞다면, 앞으로 누군가 `closure` 의 개념에 대해 묻는다면 SICP 의 Chapter 3.2 를 추천해주고 싶다.
   