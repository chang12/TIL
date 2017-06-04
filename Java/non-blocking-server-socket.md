간단한 코드로, 특정 포트에 바인딩해서 클라이언트의 요청을 기다리는 `ServerSocketChannel` 을 만들어본다. 이때 블로킹 여부에 따라 동작이 어떻게 달라지는지를 간단하 확인해본다.

### blocking

`ServerSocketChannel` 인스턴스를 획득하고 별도의 설정이 없다면 블로킹으로 설정된다.

```java
ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
InetSocketAddress address = new InetSocketAddress("localhost", 1234);

serverSocketChannel.bind(address);

System.out.println("server happen to be started.");
while (true) {
    serverSocketChannel.accept();
    System.out.println("client accepted.");
    Thread.sleep(1000);
}

// server happen to be started.
// 클라이언트의 요청이 있을때까지 블로킹 된다.
```

### non-blocking

`configureBlocking` 메서드를 호출해서 논-블로킹으로 설정할 수 있다.

```java
...
serverSocketChannel.bind(address);
serverSocketChannel.configureBlocking(false);
...

// server happen to be started.
// client accepted.
// client accepted.
// client accepted.
// ...
```