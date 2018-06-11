# Purpose
* 인스턴스 생성을 막는다.
* 인스턴스를 생성하려고 할 경우, 컴파일 에러를 발생시킨다.

# Rule
```java
abstract class Abstract {
	...
	abstract void showBasicInfo();
}
```

* 정의할 때 `abstract` 키워드를 앞에 붙인다.
* 메소드 앞에도 `abstract` 키워드를 붙일 수 있다. 그 경우 메소드 정의에 `{..}`의 몸체가 생략된다. 
* `abstract` 클래스라도 `abstract` 메소드를 가지지 않을 수 있다.
* `abstract` 메소드를 하나라도 가진 클래스는 `abstract` 여야 한다.
* 다른 클래스가 `abstract` 클래스를 상속하는 경우 `abstract` 메소드를 모두 구현해서 `concrete` 클래스가 되거나, 자신도 `abstract` 클래스로 정의되야한다. 

# Reference
* [난 정말 JAVA를 공부한 적이 없다구요](http://book.naver.com/bookdb/book_detail.nhn?bid=6056781) 책의 Chapter 17