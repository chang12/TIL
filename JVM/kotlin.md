## Nullable types and Non-Null Types ([link](https://kotlinlang.org/docs/reference/null-safety.html#nullable-types-and-non-null-types))

kotlin 의 type system 은 `NullPointerException` 을 제거하는걸 목표로 삼고 있다. 그래서 reference 를 nullable reference 와 non-null reference 로 구분한다. 기본적으로 type 을 정의하면 모두 non-null 인것 같고, 뒤에 `?` 을 붙이면 nullable 하게 쓸 수 있다.

nullable type 으로 변수를 선언한 경우, 본래 type 의 method 들을 자유롭게 호출할 수 없다. 호출하기위한 몇가지 방법 중 하나는 `b.length` 대신 `b?.length` 처럼 `?` 을 붙이고 method 를 호출하는건데 이를 `safe call` 이라고 부른다. safe call 의 return type 도 역시 본래 return type 에 대한 nullable type 이다.

jackson 을 사용해서 json -> class 로 deserialize 할때 이 nullable / non-null type 관해서 이슈가 있었다.

* `Int`, `Boolean` type 으로 field 를 선언했을때, json 에 대응하는 property 가 없는 경우, non-null type 이었다면 default 값 (0, false) 으로 deserialize 하고, nullable type 일때는 null 로 deserialize 한다.
* `String` type 의 field 는 nullable type 일때는 마찬가지로 null 로 deserialize 되지만, non-null type 일때는 `MissingKotlinParameterException` 을 throw 한다.