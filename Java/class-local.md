# Definition
* 메소드 내에서 정의된 클래스

# Rule
* 정의된 메소드내에서만 **인스턴스 생성**, **참조변수 선언**이 가능
* 정의된 메소드의 **지역변수**, 메소드로 전달된 **argument**에 접근할 수 있다.
	* **argument**는 **final** 로 선언되어야 한다.
	* JVM이 복사본을 Local 클래스가 항상 접근가능한 메모리 영역에 만들어두는 덕분이다. 
	* 그러므로 변경을 막기 위해 **final**로 선언되어야 한다.
* 생성을 요청하는 쪽에서 참조변수로 접근할 수 있도록, 어떤 **interface** 를 구현하도록 정의된다
	* 그래서 보통 Local 클래스를 사용하고 싶은 경우에는 이를 위한 **interface** 를 정의한다.

# Reference
* [난 정말 JAVA를 공부한 적이 없다구요](http://book.naver.com/bookdb/book_detail.nhn?bid=6056781)
* Chapter 17
