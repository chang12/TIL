eincs 님의 [JAVA NIO의 ByteBuffer와 Channel로 File Handling에서 더 좋은 Perfermance 내기!](http://eincs.com/2009/08/java-nio-bytebuffer-channel-file/) 포스트를 읽었다. `java.io` 패키지를 사용한 Java I/O 작업이 왜 성능이 떨어지는지 그 이유를 알려주는 유익한 포스트다.

### Java I/O 는 CPU overhead 가 존재

[read (system call) Wikipedia 문서](https://en.wikipedia.org/wiki/Read_(system_call)) 를 보면 `calling process` 가 제공하는 `buffer` 로 bytes 를 읽어들인다고 적혀있다. 블로그 포스트와 함께 이해해보면, `calling process` 는 `kernel` 의 process 를 의미하는 듯 하다. 그러므로 `kernel` 이 내부에 `buffer` 를 들고 있고, `buffer` 로 bytes 를 읽어들이는 것은 DMA 에 의해서 CPU overhead 없이 이뤄진다.

그러나 기존의 `java.io` 에서는 Java 코드에서 `kernel` 이 들고있는 `buffer` 에 직접 접근할 수 있는 방법이 없었다. 그러므로 `kernel` 이 들고있는 `buffer` 에서 JVM 이 들고있는 `buffer` 로 bytes 를 복사해와야하고, 이 복사 작업에는 CPU 가 관여하게 된다. 그러므로 C/C++ 같은 언어에서는 없앨 수 있는 **CPU overhead 가 존재한다.**

### Java I/O 는 blocking

`kernel` 이 들고있는 `buffer` -> JVM 이 들고있는 `buffer` 로 bytes 를 복사하는 작업이 이뤄지는 동안 해당 I/O 작업을 요청한 thread 가 blocking 된다. **그러므로 blocking I/O 인 것이다.** 이는 성능상으로 아쉬운 점이다. 
