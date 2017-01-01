`Service` 인터페이스는 operational state 를 지닌 객체를 표현하는데 적합하다. web server, RPC server 들이 그 예시.

* startAsync, stopAsync 메서드로 비동기적인 시작/종료가 가능하다.
* addListener 메서드로 asynchronous 하게, awaitRunning / awaitTerminated 메서드로 synchronous 하게 state transition 에 method 를 invoke 할 수 있다.

`Service` 인터페이스를 직접 구현하는 것보다, 몇개의 Abstract class 들 중 목적에 맞는걸로 선택해서 필요한 부분만 구현하기를 권장하고 있다. 또한 Service 들의 collection 이 필요하다면 이를 직접 다루는것보다 `ServiceManager` 클래스를 활용해서 다루기를 권장한다.

문서를 한번 읽어봤는데, 아직까지는 어디에 쓰일 수 있을지 가늠이 안된다. `Worker` 를 구현해야할때 바닥부터 구현하기보다, `Service` 인터페이스를 구현한 abstract class 들 중에서 하나를 골라 상속하는 방법을 쓸 수 있을까?