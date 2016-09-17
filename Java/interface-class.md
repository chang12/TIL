# Definition
어떤 클래스의 모든 메소드가 `abstract` 라면, 그 클래스는 **interface** 클래스이다.

# Rule
* **interface** 클래스의 모든 변수는 `public static final`
	* `constant` 들을 정의할 때 행사코드가 줄어든다.
	* **JDK 5.0** 부터 **enum** 이 지원되면서 대체되었다.
* **interface** 클래스의 모든 베소드는 `public abstract`
* 인스턴스를 생성할 수 없다.
* 참조변수를 선언할때 타입으로는 사용할 수 있다.

# Use
* 클래스 설계에 유용하다. 설계와 구현을 분리할 수 있다.
	*  어떤 클래스가 필요할 때, 해당 클래스의 **interface** 를 먼저 설계해서 협력하는 팀원에게 넘긴다.
	*  팀원이 클래스를 구현하는 동안, 구현체를 가정하고 자신의 파트에 집중할 수 있다.
* 다중 상속이 필요할때 사용할 수 있다.
	* `implements` **interface1**, **interface2** ...

# Reference
* [난 정말 JAVA를 공부한 적이 없다구요](http://book.naver.com/bookdb/book_detail.nhn?bid=6056781)
* Chapter 17
