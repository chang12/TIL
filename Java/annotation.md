annotation processor 에 관해 잘 쓰여진 블로그 글을 읽기에 앞서 [Oracle 의 Java 공식 문서의 내용을](http://docs.oracle.com/javase/tutorial/java/annotations/index.html) 간단히 짚어봤다.

### Java 8 부터 제공해주는 추가 기능

* **repeated annotation** 을 지원한다. 같은 annotation 을 하나의 타겟에 여러개 달 수 있다. 따로 [관련 문서도](http://docs.oracle.com/javase/tutorial/java/annotations/repeating.html) 존재한다.
* 기존의 annotation 은 declaration 에만 달 수 있었다. 하지만 생성자 호출 / 형변환 / `implements` 구문 / `throws` 구문에도 달 수 있게 확장되었다. 이들을 묶어서 **type annotation** 이라고 부른다. 마찬가지로 따로 [관련 문서가](http://docs.oracle.com/javase/tutorial/java/annotations/type_annotations.html) 존재한다. 공식 문서에서는 이들을 활용해서 type checking 을 위한 pluggable module 을 만들 수 있다고 소개하면서, [워싱턴 대학에서 만든 Checker Framework](https://checkerframework.org/) 를 소개한다.

### Terminology

* annotation 정의할때 원소 선언하는걸 **annotation type element declaration** 이라고 부른다.
* 다른 annotation 정의에 쓰일 수 있는 annotation 을 **meta annotation** 이라고 부른다.