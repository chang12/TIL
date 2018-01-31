## case class 를 serialization

case class 의 camel case 필드 이름들을 snake case 로 바꿔주는 방법을 찾아봤다.

```scala
import net.liftweb.json.compactRender
import net.liftweb.json.DefaultFormats
import net.liftweb.json.Extraction.decompose
import net.liftweb.json.JsonAST.JField
import net.liftweb.util.StringHelpers

case class Person(firstName: String, lastName: String)

val p = Person("Max", "Lee")

implicit val formats: DefaultFormats.type = DefaultFormats
    
val ser = compactRender(
	decompose(p)
		.transformField {
			case JField(n, v) => JField(StringHelpers.snakify(n), v)
		}
)

println(ser)
```