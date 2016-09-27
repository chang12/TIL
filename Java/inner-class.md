# Definition
* 클래스 속 클래스
* `static` 으로 정의되는 경우 = **static inner** 혹은 **nested**
* 아닌 경우 = **inner**

# Rule
## Nested
```java
OuterClass.NestedClass nst = new OuterClass.NestedClass();
```
## Inner
```java
OuterClass out = new OuterClass();
OuterClass.InnerClass in1 = out.new InnerClass();
OuterClass.InnerClass in2 = out.new InnerClass();
```

* **inner** 인스턴스는 **outer** 인스턴스에 종속된다.
* 하나의 **outer** 인스턴스에 여러개의 **inner** 클래스가 존재할 수 있다.


# Reference
* [난 정말 JAVA를 공부한 적이 없다구요](http://book.naver.com/bookdb/book_detail.nhn?bid=6056781)
* Chapter 17
