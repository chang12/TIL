## Extensions ([link](https://kotlinlang.org/docs/reference/extensions.html#extensions))

jackson 의 `ObjectMapper` 으로 json string 을 deserialize 하려는데, 본래 `ObjectMapper` 에 존재하지않는 signature 의 `readValue` method 를 사용할 수 있었다. 어떻게 된건가 navigate 해보니, 다양한 `readValue` fun 들이 선언되어있었다.

```kotlin
// com.fasterxml.jackson.module.kotlin.Extensions.kt

inline fun <reified T: Any> ObjectMapper.readValue(src: File): T = readValue(src, jacksonTypeRef<T>())
inline fun <reified T: Any> ObjectMapper.readValue(src: URL): T = readValue(src, jacksonTypeRef<T>())
// ...
```

신기해서 찾아보니 kotlin 이 제공하는 **Extensions** 중 **extension functions** 을 선언한거였다. 편리해보인다. 문서에서는 `extend a class with new functionality without having to inherit from the class.` 라고 표현했다.

## Nullable types and Non-Null Types ([link](https://kotlinlang.org/docs/reference/null-safety.html#nullable-types-and-non-null-types))

kotlin 의 type system 은 `NullPointerException` 을 제거하는걸 목표로 삼고 있다. 그래서 reference 를 nullable reference 와 non-null reference 로 구분한다. 기본적으로 type 을 정의하면 모두 non-null 인것 같고, 뒤에 `?` 을 붙이면 nullable 하게 쓸 수 있다.

nullable type 으로 변수를 선언한 경우, 본래 type 의 method 들을 자유롭게 호출할 수 없다. 호출하기위한 몇가지 방법 중 하나는 `b.length` 대신 `b?.length` 처럼 `?` 을 붙이고 method 를 호출하는건데 이를 `safe call` 이라고 부른다. safe call 의 return type 도 역시 본래 return type 에 대한 nullable type 이다.

jackson 을 사용해서 json -> class 로 deserialize 할때 이 nullable / non-null type 관해서 이슈가 있었다.

* `Int`, `Boolean` type 으로 field 를 선언했을때, json 에 대응하는 property 가 없는 경우, non-null type 이었다면 default 값 (0, false) 으로 deserialize 하고, nullable type 일때는 null 로 deserialize 한다.
* `String` type 의 field 는 nullable type 일때는 마찬가지로 null 로 deserialize 되지만, non-null type 일때는 `MissingKotlinParameterException` 을 throw 한다.

## Reified type parameters

jackson 의 `ObjectMapper` 로 deserialize 할때 `mapper.readValue<XYZ>("...")` 로 할 수 있어서 편리했다. java 였다면 `mapper.readValue("...", XYZ.class)` 로 했을텐데 말이다. 그래서 찾아보니 저렇게 method 를 호출하면서 `<T>` 를 덧붙여서 명시한 type 을 **reified type parameter** 라고 부른단다.

## Bytecode 까보기

[Ditto Kim 님의 Kotlin에서 JPA 사용할 때 주의할 점](https://blog.sapzil.org/2017/11/02/kotlin-jpa-pitfalls/) 포스트에 IntelliJ 에서 kotlin class 의 bytecode 를 보는 방법이 적혀있다. `public <init>` 으로 검색해서 어떤 constructor 들이 만들어졌는지 확인할 수 있다.

```
// access flags 0x1
public <init>(Ljava/lang/Integer;Ljava/lang/String;)V
  @Lorg/jetbrains/annotations/Nullable;() // invisible, parameter 0
  @Lorg/jetbrains/annotations/Nullable;() // invisible, parameter 1
 L0
  LINENUMBER 3 L0
  ALOAD 0
...
```

# lateinit

[Late-Initialized Properties and Variables](https://kotlinlang.org/docs/reference/properties.html#late-initialized-properties-and-variables) 를 읽으면 된다. non-null type 의 property 를 constructor 에서 initialize 하기 힘들때 사용할 수 있다.

# Data Class's Copy

data class 의 `copy` 메서드가 유용하다. [Copying](https://kotlinlang.org/docs/reference/data-classes.html#copying)