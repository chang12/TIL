* [The Problem with Securing Single Page Applications](https://stormpath.com/blog/secure-single-page-app-problem)
* [Token Authentication for Java Applications](https://stormpath.com/blog/token-auth-for-java)

## Context
RESTful API 를 만들때 access token 을 쓰게 될텐데 이를 어떻게 만들지에 대한 의문이 시작이었다. access token 에 어떤 데이터를 내포시킬지, 인코딩/디코딩은 어떻게 할 것인지, 인코딩/디코딩할때 필요한 signature 같은게 어떤 것들이 있을지 궁금했다.

## Basic Authentication

* 기본적인 username / password authentication 에 대해서 얘기한다. 서버에서는 username / password 가 일치하면 고유한 **session** 을 만들고, **session id** 를 HTTP response 에 `Set-Cookie` 헤더로 넣어준다. 이를 받은 클라이언트는 이후 매 HTTP request 의 `Cookie` 헤더에 받은 값을 넣어서 요청을 보낸다. `session=da03Wa3Kl...` 같은 식으로 표현된다. 서버에서는 session id 를 key 로 다양한 user data 들을 저장소에 넣어두고 유저를 식별할때 사용할 수 있다.
* Django 할때 등장했던 CSRF token 은 **synchronizer token** 의 개념이었다. SPA 의 경우 form 이 담긴 HTML 문서가 이미 컴파일되어 클라이언트단이 가지고 있으므로, CSRF token 을 form 의 hidden field 로 삽입할 수 없기 때문에 synchronizer token 방법은 불가능하다.
* **same origin policy** 는 브라우저가 제공하는 메커니즘이다. 어떤 스크립트가 어떤 endpoint 와 상호작용하기 위해서는, 그 endpoint 가 그 스크립트를 제공한 endpoint 와 동일한 base URL 을 가져야한다는 메커니즘이다.
* **double submit cookie** 방법에서는 로그인할때 서버가 클라이언트에게 cookie 를 두개 내려준다. 하나는 보통 우리가 얘기하는 cookie 이고, 다른 하나는 랜덤하게 생성된 값이다. 클라이언트는 로그인 후 매 요청마다 `Cookie` 헤더와 함께 `X-XSRF-Token` 같은 커스텀 헤더에 두번째 cookie 값을 넣는다. 만약 해커가 CSRF 를 시도하는 경우에는 (서버가 받아들일) 두번째 랜덤 cookie 값을 제공할 수 없으므로 공격할 수 없다.
* **origin header** 를 이용하는 방법은 브라우저의 도움을 받는다. modern 브라우저에서는 Javascript 코드로 `Origin` 헤더값을 설정하는걸 금지한다. 그러므로 서버에서 요청을 받으면, `Origin` 헤더값을 확인해서 서버가 소유한 base URL 과 일치하는지 검증할 수 있다.
* web server 인스턴스가 여러대가 되면, session id 와 거기에 물린 user data 를 관리하는게 challenging 해진다. 모든 인스턴스간에 동기화된 session de-referencing service 가 필요해지기 때문이다. 또한 근본적으로 session id 는 unique identifier 의 역할만 할뿐, 그 자체에는 아무런 정보도 담겨있지 않다. 이 포인트에서 **token authentication** 의 효용성이 대두된다.

## Token Authentication

* **JSON Web Tokens(JWTs)** 에 대해서 생각한다.
* Token Authentication 과 Basic Authentication 의 가장 큰 차이는 token 이 정보를 지닌다는 점이다. Basic Authentication 에서 session id 는 unique identifier 역할만 할뿐 그 자체로는 아무런 정보를 지니지 않았다. 그러므로 서버에서 session id - user data 매핑을 들고있어야했고, 분산 서버환경에서 이는 challenging 해진다. 이에 비해 JWT 는 유용한 정보들이 **encrypted** / **encoded** 된 결과물이다.
* JWT 는 period(.) 를 기준으로 여러 **part** 로 이뤄져있다(각 part 는 **base64 encoded**). **header** / **claims** / **cryptographic signature** 들이다. 각 part 들이 내장한 정보들에 대해서는 [JWT specification](https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32) 문서에 아주 자세하게 적혀있을 것이다.
* header 에는 typ 과 alg 필드가 있다. **typ** 에는 "JWT" 처럼 token 의 종류가, **alg** 에는 "HS256" 처럼 사용한 알고리즘의 종류가 있다. 
* claims 에 정보들이 많이 있다. **iss** 에는 `"http://trustyapp.com/"` 처럼 token 을 발급한 곳의 정보가, **exp** 에는 token 의 기한이 unix timestamp 값으로, **sub** 에는 해당 token 에 대응되는 user 의 식별자가, **scope** 에는 해당 token 이 지니는 permission 이 기술되어있다. 이 중 앞의 3개는 JWT specification 에 기술된 항목이고, 마지막 scope 는 아니다. 이렇게 필요에 의해 추가적인 필드들을 포함시켜서 인코딩해도 무방하다.
* JJWT 라이브러리로 작성된 코드 snippet 을 보면, sign 하는 과정에 **algorithm** / **signature key** 두 가지가 개입한다. signature key 는 byte array 타입이었는데, 아마 임의로 생성한 뒤에 서버에서 철저한 보안하에 보관하고 있어야하는 값일 것으로 보인다. 이 내용과 cryptographic signature 가 어떻게 연관되는지에 대해서는 추가적으로 파악해야할 내용이다.

## JWT 의 한계점

PyCon KR 2017 에 가서 [PHP에서 Django로 갈아타기](https://www.pycon.kr/2017/program/171) 세션을 들었다. PHP 와 Django 가 백엔드에 혼재되었을때 인증 절차를 공유하기 위해서 별도의 MySQL DB 를 두는 방식으로 해결했다는 얘기로 마무리 되었다. 질문 시간에 JWT 를 사용하는 방법은 고려해보지 않았는가 물어봤다. 세션이 끝나고 발표자분과 2층으로 이동하면서 더 대화를 나눴고, 그 과정에서 JWT 에 대해 더 생각해보게 되는 계기가 되었다.

[Stop using JWT for sessions](http://cryto.net/~joepie91/blog/2016/06/13/stop-using-jwt-for-sessions/) 이라는 글을 읽었고, JWT 를 인증을 위한 token 으로 사용했을때의 문제점들을 몇가지 지적했다. [Stop using JWT for sessions, part 2: Why your solution doesn't work](http://cryto.net/~joepie91/blog/2016/06/19/stop-using-jwt-for-sessions-part-2-why-your-solution-doesnt-work/) 라는 후속글도 있었다. 

* JWT 로 만들어진 token 이 cookie storage 에 저장된다면 아무런 문제가 없다. 정상적인 브라우저들은 모두 JS 코드를 통한 cookie storage 접근을 막아주기 때문이다. (그런데 Chrome 개발자 도구에서 `document.cookie` 하면 뭔가 값이 찍힌다?) 그러나 cookie storage 는 4KB 의 용량 제한이 있고, JWT 에 더 많은 데이터를 담을 수록 이 제한을 넘칠 가능성이 높아진다. 대안으로 local storage 를 사용할 경우 공격받을 위험이 커진다. (여담으로 [User Authentication With Angular 4 and Flask](https://realpython.com/blog/python/user-authentication-with-angular-4-and-flask/) 라는 블로그 포스트를 읽었는데, 여기서도 local storage 에 JWT 를 저장했다)
* malformed session 을 인식하더라도 JWT 에서는 해당 session 을 invalidate 할 수 없다. JWT 을 이용한 인증 절차에서 token 이 invalidate 되는 유일한 시점은 expiry 가 끝나는 경우다. 하지만 한번 발급한 token 의 expiry 는 수정할 수 없기 때문에 즉각적인 invalidation 이 불가능하다. 

## 그렇다면 OAuth 2.0 에서는?

그러던 중 문득 `access_token` 을 적극 활용하는 OAuth 2.0 프로토콜(?)의 경우에는 어떤 방식으로 토큰을 발급하는지 궁금해졌다. 출처를 다시 찾지못한 어떤 Stack Overflow 답변에서는 OAuth 2.0 표준에서 어떤 토큰을 사용해야하는지에 대해서는 상세하게 언급하지 않는다고 설명하며, 어떤 방식의 토큰이던 OAuth 2.0 프로토콜에 끼워봤을때 문제가 없다면 사용가능하다고 얘기했다. 아래 주제들을 더 읽어봐야겠다.

* [JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants](http://self-issued.info/docs/draft-ietf-oauth-jwt-bearer.html?cm_mc_uid=95364242177615045684830&cm_mc_sid_50200000=1504568483) -> `Bearer` 토큰이 뭔지 이해하고 싶다.
* [Cookies vs Tokens: Definitive Guide](https://auth0.com/blog/cookies-vs-tokens-definitive-guide/) -> 여담으로...