JSON 을 처리할때 [lift-json](https://github.com/lift/lift/tree/master/framework/lift-base/lift-json) 라이브러리를 사용한다. 아래와 같은 패턴을 늘 반복한다.

```scala
{
    implicit val formats = net.liftweb.json.DefaultFormats
    val name = (person.json \ "who" \ "name").extractOpt[String].getOrElse("")
    ...
}
```

이때 `implicit val` 부분을 안적으면 에러가 발생한다. 이 부분에 대해서 궁금해서 [Scala Doc 의 implicit parameters](https://docs.scala-lang.org/ko/tutorials/tour/implicit-parameters.html.html) 부분을 읽어봤다. 명시적으로 파라미터를 넘겨주지 않더라도, 이전에 `implicit val` 키워드로 선언된 타입이 일치하는 immutable 이 있다면 이를 사용하는 컨셉이다. 문서를 차분히 읽어보니 이해가 갔다. 간단한 예제를 작성해보고 REPL 에서 실습해봤다.

```scala
implicit val a = "max"

def concat(s: String)(implicit prefix: String) = {
    s"${prefix}_${s}"
}

concat("ability")("min")  // res0: String = min_ability
concat("ability")  // res1: String = max_ability
```