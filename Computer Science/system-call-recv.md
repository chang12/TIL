gather / scatter 의 개념을 정리하고나서 `select` system call 과의 관계를 알아보기 위해서 "gather scatter select system call" 이라는 검색어로 구글링을 했다. 결과중에 `recv` system call 의 [리눅스 문서가](http://www.tutorialspoint.com/unix_system_calls/recv.htm) 링크되어있어서 읽어봤다. 리눅스 문서는 정말 꼼꼼히 읽어지지가 않는다. 그래도 눈에 띄는 부분이 있었다.

> If no messages are available at the socket, the receive calls **wait for a message to arrive, unless the socket is nonblocking** (see fcntl(2)),

어제 `read` system call 에 대해서 찾아봤을때, SO 포스트에서 `read` system call 의 경우 file 맥락에서는 읽을 data 가 없을 경우 바로 반환되지만 socket / pipe 맥락에서는 data 가 도착할때까지 blocking 된다고 했었다. 이에 비해서 `recv` system call 의 경우는 socket 의 특성에 따라 달라진다. 자연스럽게, **socket 을 명시적으로 non-blocking 으로 만드는 방법이 따로 있겠구나** 라는 생각이 든다.