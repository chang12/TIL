## Headers

`accept:application/json` 으로 주면 response body 를 json 으로 받을 수 있다. 헷갈려서 `content-type:application/json` 을 줘놓고 왜 body 가 json 이 아니라 protobuf 일까 1시간 고민한적이 있었다. `content-type` 은 post request 의 body 를 어떻게 줄지 선언하는 것이다.