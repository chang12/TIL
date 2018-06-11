Scala 의 `implicit class` 구문을 사용해서 기존에 정의된 클래스에 새로운 메서드를 정의한 것과 동일한 효과를, 기존 클래스의 수정없이 얻을 수 있다. 라이브러리의 클래스라 수정이 불가능한 경우에 유용할 것이다.

[A Scala 2.10 implicit class example (how to add new functionality to closed classes](https://alvinalexander.com/scala/scala-2.10-implicit-class-example)

위의 블로그 포스트를 보면 이해하기 쉬운 친절한 예제로 잘 설명해준다. 아래와 같이 정리해볼 수 있다.

* `class A` 는 이미 정의되어있다.
* 생성자에서 `A` 인스턴스를 파라미터로 받는 `class B` 를 정의한다.
* `class A` 에 추가하고 싶은 메서드의 기능을, `class B` 에 구현한다. (`methodForA`)
*  `a.methodForA` 를 호출하면 실제 `A` 에는 해당 메서드가 없음에도 실행된다.

Scala REPL 에서 블로그 포스트의 예제를 실행했을때 잘 동작하는 것을 확인했다. 포스트에도 나와있듯이, 이를 REPL 이 아니라 프로젝트 레벨에서 사용하려면 `implicit class` 를 `object` 혹은 `package object` 에 잘 포함시키고, 사용하려는 곳에서 적절히 import 해줘야 한다.
