[JVM Wikipedia](https://en.wikipedia.org/wiki/Java_virtual_machine)

* JVM 에는 **specification** 이 있다. specification 은 JVM 이 갖춰야할 명세서다. 그러므로 서로 다른 implementation 도 같은 specification 을 이 specification 을 고려해서 작성된 bytecode 는 모두 호환가능하다.
* JRE 는 **Java Runtime Environment** 의 약자이다. software package 이다. 이는 JVM 과 **Java Class Library** 를 포함한다. Oracle 이 배포하는 JRE 에 들어있는 JVM 을 **HotSpot** 이라고 부른다.
* JDK 는 JRE 에 **javac compiler** 같이 개발자들이 사용할만한 도구들을 더 포함시킨 패키지다.
* [Class loader](https://en.wikipedia.org/wiki/Java_Class_loader) : JVM Wikipedia 에 나와있는 내용만 봐서는 잘 모르겠다. **bootstrap class loader** 와 **user defined class loader** 의 두 가지 종류가 있는데, JVM implementation 이 bootstrap class loader 를 규정하므로 이건 꼭 갖춰야한다.
* [Bytecode instructions](https://en.wikipedia.org/wiki/Java_bytecode) : Java core API 들을 호스트 OS 에 맞게 효율적으로 구현하는 것이 중요하다.
* class file 은 byte code 와 symbol table 을 포함한다. class file format 은 HW 나 OS 에 독립적인 binary format 이다.
* [JIT compiler](https://en.wikipedia.org/wiki/Just-in-time_compilation) 얘기가 나오는데 맥락을 정확히 이해하지 못하겠다. 완벽하게 native machine language 로 컴파일 되서 실행되는 방법, bytecode interpreter 에 의해서 실행되는 방법이 있는건 알겠는데, JIT compiler 에 의해서 실행중에 native machine language 로 compile 되며 compile 된 부분은 실행된다는게 interpreter 와 어떻게 다른건지 잘 모르겠다.
* [JavaPoly](https://en.wikipedia.org/wiki/Java_virtual_machine#cite_ref-4) 를 사용하면 클라이언트 사용자의 머신에 Java 가 설치되어이지 않더라도, unmodified Java library 를 사용할 수 있다는데 대체 어떻게 가능할지 의아하다.