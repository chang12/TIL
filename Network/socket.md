### server
```java
ServerSocket serverSocket = new ServerSocket(5000); // port 번호만 지정하는 생성자. localhost 에 만들어진다.
Socket socket = serverSocket.accept(); // 요청이 들어와서 accept 할때까지 blocking
```
얻어낸 socket 인스턴스에서 `InputStream` / `OutputStream` 들을 뽑아내서 사용하면 된다.

### client
```java
Socket socket = new Socket("127.0.0.1", 5000) // host 문자열과 port 번호를 지정하는 생성자.
```
server 와 마찬가지로, 생성한 socket 인스턴스에서 `InputStream` / `OutputStream` 들을 뽑아내서 사용하면 된다.

HTTP 프로토콜단과 차이점은 클라이언트가 서버에 요청해서 데이터를 받아오는 일방적인 관계가 아니라, 클라이언트 서버가 서로 양방향 통신을 할 수 있다는 점이다. 클라이언트 socket 의 `OutputStream` 에 write 하면, 서버 socket 에서 얻은 socket 인스턴스의 `InputStream` 에서 read 할 수 있고 그 반대도 가능하다. 그러므로 채팅 같은 어플리케이션을 구현할 수 있는 것이다.