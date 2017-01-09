## Reference
* [AngularJS Developer Guide : Providers]()

angular module 을 더 modular 하게 설계하고 싶다. 의미적/행동적으로 구분되는 단위별로 object 를 개발하고, object 들을 쌓아올려서 controller 를 만들고, controller 들을 합쳐서 module 을 만들고 싶은 마음이다.

AngularJS 에서는 모든 작업이 **injector** 를 통해서 이뤄진다. injector 에게 object 를 만드는 방법을 등록하고, 다른 object 에서 받아다 쓰고싶다면 injector 에게 요청한다.

그러므로 우선 injector 에게 object 를 만드는 방법을 알려줘야 한다. 방법을 **recipe** 라고 표현했다. AngularJS 에는 5가지 recipe 유형이 존재한다. **provider** / **value** / **factory** / **service** / **constant** 들이다.

모든 recipe 들은 (object identifier, object recipe) 쌍으로 module 에 등록된다는 사실을 명심하자.

## Value Recipe
module 내에서 controller 들 사이에 공유되어야 할 설정값 같은게 있다면 유용하게 쓸 수 있겠다.