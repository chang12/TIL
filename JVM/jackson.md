# Annotations

## JsonIgnoreProperties

```java
@JsonIgnoreProperties(ignoreUnknown = true)
class A {
    ...
}
```

json -> class 로 deserialize 할때 `ignoreUnknown` 의 값을 true 로 선언할 수 있다 (default = false). json property 중에서 class field 로 mapping 할 수 없는 녀석들이 있을때, `ignoreUnknown` = false 라면 `UnrecognizedPropertyException` 를 throw 하고, `ignoreUnknown` = true 라면 무시하고 넘어간다.