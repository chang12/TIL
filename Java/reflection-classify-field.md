`Class` 인스턴스의 getDeclaredFields() 메서드를 호출해서 access identifier 에 무관하게 모든 필드들의 정보를 받아올 수 있다.
```java
Class clazz = 클래스_이름.class;
for (Field field : clazz.getDeclaredFields()) {
	...
}
```
## Field 타입이 enum
메서드가 있어서 아주 편리하다.
```java
Field field = ...
if (field.getType.isEnum()) {
	...
}
```
## Field 타입이 Map
```java
Field field = ...
if (Map.class.isAssignableFrom(field.getType())) {
	ParameterizedType parameterizedType = (ParameterizedType) field.getGenericType();
	Type[] actualTypes = parameterizedType.getActualTypeArguments(); // Map 이라면 길이=2
	Class keyType = (Class) actualTypes[0];
	Class valueType = (Class) actualTypes[1];
	...
}
```
## Field 타입이 Collection
```java
Field field = ...
if (Collection.class.isAssignableFrom(field.getType())) {
	ParameterizedType parameterizedType = (ParameterizedType) field.getGenericType();
	Type[] actualTypes = parameterizedType.getActualTypeArguments(); // Collection 이라면 길이=1
	Class componentType = (Class) actualTypes[0];
	...
}
```
## Field 타입이 Array
enum 과 마찬가지로 메서드가 있어서 아주 편리하다.
```java
Field field = ...
if (field.getType().isArray()) {
	Class componentType = field.getType().getComponentType();
	...
}
```
## Field 타입이 Java 기본 자료형 여부 파악
Java 기본 자료형을 크게 3가지 유형으로 나눠서 생각할 수 있다.
* **primitive type** ex) int, char, double ...
* **primitive type wrapper** ex) Integer, Character, Double ...
* **String**

다양한 방법이 가능하겠지만, 3가지 유형을 다합쳐도 몇가지 안되므로 모든 경우의 수를 준비해놓고 해당하는게 있는지 찾는 방법이 가능하다. 각각의 타입을 나타내는 `Class` 인스턴스의 getName() 메서드 반환값을 모두 고려할 수 있다.
* **primitive type** -> "int", "char", "double" ...
* **primitive type wrapper** -> "java.lang.Integer", "java.lang.Character", "java.lang.Double" ...
* **String** -> "java.lang.String"

그러므로 사전에 `HashMap` 등을 준비해서 확인할 수 있을 것이다.