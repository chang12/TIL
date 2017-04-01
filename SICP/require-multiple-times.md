아래 구조와 같은 require 관계를 가정한다.

```
             top
           /     \
        left     right
           \     /
           bottom
```

`top` 에서는 procedure 를 정의하고, 이를 실행한다. `left` 와 `right` 는 `top` 을 require 하므로 각각의 코드를 실행할 경우 `top` 에 정의된 procedure 가 실행된다. 이때 `left` 와 `right` 를 둘 다 require 하는 `bottom` 에서는 `top` 에 정의된 procedure 가 2번 실행될지가 의문이었다.

```racket
; top.rkt
(define (test)
    (print "test"))

(test)

; left.rkt
(require "top.rkt")

; right.rkt
(require "top.rkt")

; bottom.rkt
(require "left.rkt")
(require "right.rkt")
```

결과적으로 `bottom` 을 실행했을때 **"test"** 문자열은 한번만 출력되었다. 그러므로 require 구문이 실질적인 import 실행 자체를 의미하는게 아니라고 유추할 수 있다. 단어 뜻 그대로 특정 코드를 필요로 한다는 선언이라고 이해할 수 있겠다. `bottom` 은 `left` 와 `right` 를 필요로하는데, `left` 에서 필요로하는 `top` 을 이미 import 했으므로, `right` 의 require 구문은 반복 실행되지 않고 넘어가는 것이다.

그러므로 중복되는 require 구문에 대한 걱정은 접어둘 수 있겠다.