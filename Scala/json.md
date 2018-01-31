## JSON string <-> Scala case class

`lift-json` 에 dependency 가 걸려있다.

```scala
import net.liftweb.json.compactRender
import net.liftweb.json.DefaultFormats
import net.liftweb.json.Extraction.decompose
import net.liftweb.json.JsonAST.JField
import net.liftweb.util.StringHelpers
```

JSON string 으로 serialization 할때는, `camel case` 로 적힌 case class 의 필드명을 `snake case` 로 바꿔준다.

```scala
def toJson(obj: Any): String = {
  implicit val formats: DefaultFormats.type = DefaultFormats
  
  compactRender(
    decompose(obj)
      .transformField {
        case JField(n, v) => JField(StringHelpers.snakify(n), v)
      }
    )
  }
```

case class 로 deserialization 할때는, `snake case` 로 적힌 JSON 필드명을 `camel case` 로 바꿔준다. 호출할때 type parameter 를 명시해준다.

```scala
def fromJson[A](json: String)(implicit mf: scala.reflect.Manifest[A]): A = {
  implicit val formats: DefaultFormats.type = DefaultFormats

  parse(json)
    .transformField {
      case JField(n, v) => JField(StringHelpers.camelifyMethod(n), v)
    }
    .extract[A]
}

// fromJson[List[String]](...)
```