Angular 1 공식 문서를 순서대로 읽어나간다. 그 도중에 기록하고 싶은 사항들을 기록한다.

* [service 문서](https://docs.angularjs.org/guide/services) 를 아직 안봤다.
* [Guide](https://docs.angularjs.org/guide) 에서 Application Structure 정도부터 보면 되지 않을까 싶다.

## view-independent 한 로직은 service 로 뽑아낸다.

문서에서는 `module` 에 `factory` 를 등록하고, `controller` 에서 이를 주입받아 사용한다. 이때 주입받는 controller 가 속한 module(m1) 과, factory 가 속한 module(m2) 이 다르다. m1 을 정의할때 m2 의 이름으로 주입받고, controller 를 정의할 때 factory 의 이름으로 주입받을 수 있다. 이 과정에서 Angular 의 의존성 주입 원칙을 한가지 알 수 있었다. HTML 파일들을 모두 compile 하면서, **ng-app** directive(?) 를 만나면 해당 module 을 생성(?)하는데 그 module 이 의존하는 다른 module 들도 함께 생성하는 것이다.

## controller 를 선언할때 넣어주는건 constructor function 이다.

> In the previous section we saw that controllers are created using a constructor function.

## $http 는 XMLHttpRequest 와 JSONP transport 의 wrapper 이다.

> $http is a wrapper around XMLHttpRequest and JSONP transports.

`XMLHttpRequest` 와 `JSONP` 의 정체가 궁금하다.