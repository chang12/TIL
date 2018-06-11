생성자 파라미터에 **default** 값을 지정할 수 있다.

```scala
case class Person(name: String, age: Int = 26)

Person("Max", 26)  // res191: Person = Person(Max,26)
Person("Max")  // res192: Person = Person(Max,26) 
```

default 값을 지정한 파라미터 뒤의 파라미터에 default 값을 지정하지 않으면 뭔가 문제가 생긴다.

```scala
case class Person(name: String = "Max", age: Int)

Person(26)  // <console>:27: error: not enough arguments for method apply: (name: String, age: Int)Person in object Person.
```

생성자를 오버로딩할 수 있다.

```scala
case class Person(name: String, age: Int) {
    def this(line: String) = this(line.split(",")(0), line.split(",")(1).toInt)
}

Person("Max", 26)  // res214: Person = Person(Max,26) 
new Person("Max,26")  // res215: Person = Person(Max,26)
```

**companion object** 의 apply 메서드를 오버로딩해서 `new` 키워드 없이 case class 를 생성하게 만들 수도 있다.

```scala
case class Person(name: String, age: Int)
object Person {
    def apply(line: String): Person = {
        val arr = line.split(",")
        new Person(arr(0), arr(1).toInt)
    }
}
```

하지만 scala shell 에서 순서대로 코드를 입력하면 warning 을 만나게 된다.

```shell
warning: previously defined class Person is not a companion to object Person.
Companions must be defined together; you may wish to use :paste mode for this.
``` 

인터프리터가 코드를 한번에 받아서 처리하도록, `:paste` 모드를 사용하면 해결된다. shell 에서 `:paste` 를 먼저 입력하면 paste mode 로 진입하고, 전체 코드를 입력하고, paste mode 에서 벗어나면 된다.

```scala
Person("Max", 26)
Person("Max,26")
```

그러나 Zeppelin 에서는 `:paste` 를 사용할 수 있는 방법이 없다. Zeppelin GitHub 에서 [이 문제를 해결하는 PR](https://github.com/apache/zeppelin/pull/780) 을 찾았고, master 에 반영되었다는데, 여전히 같은 문제를 겪는 2명의 사람들의 코멘트가 있었다. PR 을 날린 사람이 후속 조치를 하겠다고 적어놨는데 감감 무소식이다.
