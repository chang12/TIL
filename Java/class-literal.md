## Motivation
* 회사 코드를 살펴보다 `.class` 로 **argument**가 전달되는 경우를 목격
* 모든 객체들이 공통으로 가지는 **field**로 추측
* 구글링 시작
* [스택오버플로우 답변](http://stackoverflow.com/questions/8836635/is-class-a-method-or-field)에 따르면 **field**가 아니고, `public static final`처럼 보이는 **built-in language feature**
* 즉 `.class`를 **Class** 객체의 인스턴스라고 이해할 수 있음

## java.lang.Class
