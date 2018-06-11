[eincs 님의 블로그](http://eincs.com/) 포스트를 읽고있다.

### ByteBuffer

`java.nio` 패키지에는 여러 `Buffer` 구현체들이 있다. 하지만 이 중에서 `kernel buffer` (사실 kernel 의 buffer 라는게 정확히 뭔지 모르는 상태다. kernel 도 결국 program 이고 process 일까...?) 에 직접 접근할 수 있는 것은 `ByteBuffer` 가 유일하다. 이를 통해 `java.io` 패키지를 사용한 I/O 에서 문제가 됬었던 CPU overhead 를 줄일 수 있다. `ByteBuffer` 인스턴스를 획득하고싶다면 2가지 팩터리 메서드를 사용할 수 있다.

```java
ByteBuffer headByteBuffer = ByteBuffer.allocate(20);
ByteBuffer directByteBuffer = ByteBuffer.allocateDirect(20);
``` 

`ByteBuffer` 는 abstract class 이므로 IDE 의 break point 를 통해 실제 만들어진 인스턴스가 어느 클래스의 구현체인지 확인해본다. 이를 이름에 명시했다. 직관적인 이름들이다. 전자는 `HeapByteBuffer` 구현체다. `kernel buffer` 의 bytes 를 JVM 프로세스의 `kernel` 에 복사해온 데이터를 접근하므로 heap 에 접근한다. 후자는 `DirectByteBuffer` 구현체다. `kernel buffer` 에 직접 접근한다는 느낌을 물씬 풍긴다.

### 네가지 포인터

`DirectByteBuffer` 클래스를 보면 (사실 `ByteBuffer` 의 부모 클래스인 `Buffer`) 내부에 `mark` / `position` / `limit` / `capacity` 네개의 `int` 변수를 품고있다. 앞 3개는 포인터로 기능하고, `capacity` 는 buffer 의 크기를 나타내는 상수값이다. `mark` 는 사용자가 임의로 지정할 수 있는 포인터로, 특정 위치를 기억하고 싶을때 유용하고 사용할 수 있다. `position` 은 read/write 가 요청됬을때 읽기/쓰기 시작할 위치다. `limit` 는 접근하면 안되는 최초의 포인터 값을 나타낸다.

### flip

TODO (FileInputStream / FileOutputStream 을 만들어 FileChannel 2개를 뽑고, ByteBuffer 로 매개한다. 이때 한쪽 FileChannel 을 통해 ByteBuffer 로 데이터를 읽어들이고, 다른 FileChannel 로 ByteBuffer 의 데이터를 내보내기전에 flip 메서드를 꼭 호출해줘야한다. 호출하지 않고 바로 내보내니 문자열의 인코딩이 깨져있었다)
