## Work Flow
서버가 GCM Server 로 메시지 전송을 요청한다.
* 정상적으로 요청되었다면 GCM Server 는 서버로 **ACK** 을 보낸다.

GCM Server 가 클라이언트로 메시지를 전송한다.
* 전송이 성공했다면, 서버에게 `DeliverReceipt` 를 전송한다.
* 서버->GCM Server 로 HTTP 요청을 보내는건 쉽지만, GCM Server->서버로 HTTP 요청을 보내는건 또 다른 이슈라는 생각을 했다.

## Introduction
[Google Developers 사이트 링크](https://developers.google.com/cloud-messaging/) 만 보면 단순히 정해진 URI 에 HTTP 요청만 마구 보내면 될 것 같다. 하지만 이렇게 간단하지 않을 것 같다는 생각에 사이트를 좀 더 뒤져본다.
```HTTP
https://gcm-http.googleapis.com/gcm/send
Content-Type:application/json
Authorization:key=AIzaSyZ-1u...0GBYzPu7Udno5aA
{
	"to": "/topics/foo-bar",
	"data": {
		"message": "This is a GCM Topic Message!",
	}
}
```
[좀 더 상세한 문서](https://developers.google.com/cloud-messaging/gcm)를 보니 더 많은 내용이 적혀있다. 참고로 GCM 은 Free 서비스이고, message 는 4KB payload 까지 가능하다고 한다. 내용을 좀 정리해봤다.
### App Server
GCM connection server 와 인터랙션 하기위해 유저가 구축한 서버를 뜻한다. **HTTP 프로토콜**을 구현했다면 클라이언트로 downstream 을 보낼 수 있다. **XMPP 프로토콜**을 구현했다면 downstream 뿐 아니라 클라이언트로부터 upstream 도 받을 수 있다.
### Credentials
#### Sender ID 
API 프로젝트에 하나 할당되는 **unique identifier**. GCM connection server 는 이를 가지고 해당 클라이언트에 message 를 보낼 수 있는 app server 인지 확인할 것이다.
#### Server key 
app server 가 google service 를 이용할 수 있는 주체인지 확인시켜주는 값이다. HTTP 프로토콜에서는 **POST 요청의 헤더**에 이를 집어넣고, XMPP 프로토콜에서는 **SASL PLAIN authentication 요청의 password** 로 사용된다. 2016년 9월부터 Firebase Console 에서만 새롭게 생성할 수 있다.
### ACK
TODO
## GCM Server 로 메시지 전송 요청
app server 의 구현을 위한 자료를 보고싶어 [더욱 자세한 문서](https://developers.google.com/cloud-messaging/server#send-msg)를 읽어봤다. client app 에서 GCM 을 사용하기 위해서는, app server 가 이미 구축되어 있어야 한다. 그러한 app server 는 아래 criteria 를 지켜야 한다.
* 클라이언트와 통신할 수 있어야 한다.
* GCM connection server 에게 적절히 포맷화된 요청을 보낼 수 있어야 한다.
* **exponential back-off** 알고리즘에 따라 재전송 할 수 있어야 한다
* server key 와 client registration token 을 안전하게 보관할 수 있어야 한다. server key 는 client code 에 절대 포함되서는 안된다.
* XMPP 의 경우, 각 MESSAGE 를 구분할 unique 한 message IDs 를 생성할 수 있어야 한다. HTTP 의 경우에는 GCM connection server 가 이를 처리해준다. 같은 sender ID 에서 XMPP message IDs 는 유일해야 한다.


## GCM Server 로 부터 DeliverReceipt 획득