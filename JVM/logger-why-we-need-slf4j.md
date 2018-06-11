요새 보는 코드의 거의 모든 클래스마다 `Logger` 가 붙어있다. 클래스의 class literal 을 인자로 넘겨서 Logger 를 받아오므로, singleton 이 아닐까 추측했다. 어느 API 인지 package 를 들여다보니, **SLF4J** 라는 이름의 프로젝트였다.

> [**Simple Logging Facade for Java (SLF4J)**](http://www.slf4j.org/)

한번 훑어봐서는 정확한 의의를 파악하지 못했다. 대략 이해하기로, 여러 Logging 라이브러리들을 wrapping 하는 인터페이스를 제공하는 라이브러리였다. *java.util.logging*, *logback*, *log4j* 등의 framework 와 binding 하는 라이브러리들을 제공한다. framework 들 중에서 어느걸 사용할지 **deployment 시점**에 결정할 수 있도록 해준다. 

프로젝트가 커지면 디버깅을 비롯한 유지보수를 위해 로그 데이터를 효과적으로 관리하고 분석하는 작업이 필요할 것이다. 코드 베이스가 커지면, 단일 규칙으로 로그를 관리하기 힘들 것이다. 그러므로 구조화된 **configuration file** 파일을 작성해서 규칙을 관리하고, 이와 연계되는 **Logging API** 가 필요할 것이다. 