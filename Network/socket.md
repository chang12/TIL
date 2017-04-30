### server
```java
// port 번호만 지정하는 생성자. localhost 에 만들어진다.
ServerSocket serverSocket = new ServerSocket(5000);
// 요청이 들어와서 accept 할때까지 blocking
Socket socket = serverSocket.accept();
```
socket 인스턴스를 획득하면 `InputStream` / `OutputStream` 들을 뽑아서 사용할 수 있다.

### client
```java
// host 문자열과 port 번호를 지정하는 생성자.
Socket socket = new Socket("127.0.0.1", 5000)
```
socket 인스턴스를 생성하고, `InputStream` / `OutputStream` 들을 뽑아서 사용할 수 있다.

### TCP 프로토콜

HTTP 프로토콜에서는 클라이언트가 서버에 요청해서 데이터를 받아오는 일방적인 관계였다. 하지만 TCP 프로토콜에서는 클라이언트 <-> 서버가 서로 양방향 통신을 할 수 있다. 클라이언트 socket 에서 `OutputStream` 에 write 하면, 서버 socket 에서 `InputStream` 에서 read 할 수 있고 그 반대로도 가능하다. 이를 통해 서로 메시지를 주고받는 채팅류의 어플리케이션을 구현할 수 있다.