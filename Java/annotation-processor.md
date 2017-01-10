http://hannesdorfmann.com/annotation-processing/annotationprocessing101

## Basic Concepts
* annotation processor 는 독자적인 JVM 상에서 실행된다.

## Annotation Processor 등록
기술한 annotation processor 로 `JAR` 파일을 생성하고 이를 javac 컴파일러에 제출해야한다. `JAR` 파일의 포맷이 링크에 설명되어있다. Google 이 개발한 `AutoService` annotation 을 사용하면 `JAR` 파일 포맷을 자동으로 만들어주므로 편리하다.
```java
@AutoService(Processor.class)
public class FactoryProcessor extends AbstractProcessor {
	...
}
```
## Element / Elements
`Element` 는 package, class, method 같은 program element 들을 나타낸다.
* `PackageElement` 예) package com.example
* `TypeElement` 예) public class Foo {...} / 메소드의 시그니처
* `VariableElement` 예) private int a;
* `ExecutableElement` 예) public foo () {}

그러므로 Java 코드도 XML 같은 구조화된 텍스트라 할 수 있다. HTML 의 DOM 과 비슷한 방식으로 이해할 수 있다. 대신 **Java 코드는 Element 들로 구조화된 것**이다.
```java
TypeElement fooClass = ... ;
for (Element e : fooClass.getEnclosedElements()) { // Foo 클래스의 child 순회
	Element parent = e.getEnclosingElement(); // parent == Foo
}
```
`Elements` 는 이러한 `Element` 를 다루기 위한 utils class 이다.
## TypeMirror / Types
`TypeElement` 는 Type 그 자체만 나타낸다. 예를 들어 `Foo` 클래스에 대한 `TypeElement` 라면 해당 클래스의 super class 가 무엇인지 등의 정보는 가지고 있지 않다. 이를 위해 `TypeMirror` 가 존재한다. 추가적인 정보를 가지고 있다.

`Types` 는 이러한 `TypeMirror` 를 다루기 위한 utils class 이다.
## Filer
이름에서 알 수 있듯이, files 를 생성한다.

Meal 인터페이스 구현하는 클래스에만 붙어야하므로 이를 검증해야할 것
