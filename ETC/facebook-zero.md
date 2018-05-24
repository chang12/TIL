[Building Zero protocol for fast, secure mobile connections](https://code.facebook.com/posts/608854979307125/building-zero-protocol-for-fast-secure-mobile-connections/) 포스트를 읽으며 메모
[구글, UDP서 HTTP제공하는 QUIC 네트워크 프로토콜 인터넷 표준으로 제안 계획](http://www.djtp.or.kr/newsletter/20150511/NEWS/[ENGADGET]/engadget0502.htm) 도 읽어볼만 하다.

* secure connection 을 위해서는 **TLS(Transport Layer Security)** 를 구현하고 사용해야한다.
* TLS 에 대해 얘기할때, RTT(Round Trip Time) 횟수를 줄이는게 관건이다.
* TLS 1.2 protocol 은 **최소 1-RTT** 가 필요하다.
* Facebook 은 자체적인 연구 끝에, TLS 1.2 protocol 을 **"거의 1-RTT"** 로 구현했다.
* 현재 사용되는 TLS 1.3 protocol 은 **0-RTT** 옵션(?) 기능(?) 을 제공한다.
* 그러나 Facebook 이 처음 0-RTT 를 생각하기 시작할때는 TLS 1.3 protocol 이 0-RTT 를 제공하지 않았다.
* Google 이 만든 **QUIC** 은 UDP 상에서 동작하는 0-RTT protocol 이다.
* Facebook 은 이와 비슷한 protocol 을 TCP 상에서 만들고 싶었다.

**"Protocol changes made to QUIC"** 부터 다시 읽으면 된다.