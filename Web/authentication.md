* [The Problem with Securing Single Page Applications](https://stormpath.com/blog/secure-single-page-app-problem)
* [Token Authentication for Java Applications](https://stormpath.com/blog/token-auth-for-java)

## Basic Authentication

* 기본적인 username / password authentication 에 대해서 얘기한다. 서버에서는 username / password 가 일치하면 고유한 **session** 을 만들고, **session id** 를 HTTP response 에 `Set-Cookie` 헤더로 넣어준다. 이를 받은 클라이언트는 이후 매 HTTP request 의 `Cookie` 헤더에 받은 값을 넣어서 요청을 보낸다. `session=da03Wa3Kl...` 같은 식으로 표현된다. 서버에서는 session id 를 key 로 다양한 user data 들을 저장소에 넣어두고 유저를 식별할때 사용할 수 있다.
* Django 할때 등장했던 CSRF token 은 **synchronizer token** 의 개념이었다. SPA 의 경우 form 이 담긴 HTML 문서가 이미 컴파일되어 클라이언트단이 가지고 있으므로, CSRF token 을 form 의 hidden field 로 삽입할 수 없기 때문에 synchronizer token 방법은 불가능하다.
* **same origin policy** 는 브라우저가 제공하는 메커니즘이다. 어떤 스크립트가 어떤 endpoint 와 상호작용하기 위해서는, 그 endpoint 가 그 스크립트를 제공한 endpoint 와 동일한 base URL 을 가져야한다는 메커니즘이다.
* **double submit cookie** 방법에서는 로그인할때 서버가 클라이언트에게 cookie 를 두개 내려준다. 하나는 보통 우리가 얘기하는 cookie 이고, 다른 하나는 랜덤하게 생성된 값이다. 클라이언트는 로그인 후 매 요청마다 `Cookie` 헤더와 함께 `X-XSRF-Token` 같은 커스텀 헤더에 두번째 cookie 값을 넣는다. 만약 해커가 CSRF 를 시도하는 경우에는 (서버가 받아들일) 두번째 랜덤 cookie 값을 제공할 수 없으므로 공격할 수 없다.
* **origin header** 를 이용하는 방법은 브라우저의 도움을 받는다. modern 브라우저에서는 Javascript 코드로 `Origin` 헤더값을 설정하는걸 금지한다. 그러므로 서버에서 요청을 받으면, `Origin` 헤더값을 확인해서 서버가 소유한 base URL 과 일치하는지 검증할 수 있다.
* web server 인스턴스가 여러대가 되면, session id 와 거기에 물린 user data 를 관리하는게 challenging 해진다. 모든 인스턴스간에 동기화된 session de-referencing service 가 필요해지기 때문이다. 또한 근본적으로 session id 는 unique identifier 의 역할만 할뿐, 그 자체에는 아무런 정보도 담겨있지 않다. 이 포인트에서 **token authentication** 의 효용성이 대두된다.

RESTful API 를 만들때 access token 을 쓰게 될텐데, access token 을 만드는 기본 원칙이(어떤 데이터를 포함시킬 것인지, 인코딩/디코딩은 어떻게 할 것인지, 인코딩/디코딩할때 필요한건 어떤게 있을지 등등) 있을까 찾아본게 시작이었다. 그러므로 이 의문을 해소하기 위해 노력하자.