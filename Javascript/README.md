## TypeScript

* [Learn TypeScript in 30 Minutes](http://tutorialzine.com/2016/07/learn-typescript-in-30-minutes/)

## Class 와 Enum

parent controller 에서 alert directive 에 새로운 status 를 set 하는 상황을 생각하는 중이었다. parent controller 는 다른 child directive 와의 상호작용 중간 중간에 적절한 status alert 를 유저에게 보여주고 싶을 것이다. parent controller --> alert directive 로 one-way binding 이라 생각할 수 있다. parent controller 가 status 를 set 하면, alert directive 는 그에 맞춰 자신의 visibility / bootstrap alert class / message 등을 설정할 것이다.

이 set 정보를 하나의 class 로 표현하면 좋을 것이고, 그 class 에서 alert class 를 지정하는 필드는 몇개의 정해진 후보군 중에서 하나를 선택하는 enum 이면 좋을 것이다. 그리고 어딘가에 정의된 class / enum 을 parent controller 와 alert directive 에서 import 해서 사용해야 할 것이다. 그런 맥락에서 Javascript 의 class / enum / import 방법이 궁금했고, 이를 해결하기 위한 가장 좋은 방법이 TypeScript 인지 확인하고 싶었다.

## Angular Document Navigation

* Angular app 을 실행하면, **template** 을 **compile** 해서 **view** 를 만든다.
* `ng-controller="SearchCtrl"` 이라는 directive 가 달린 HTML tag 를 compile 하는 시점에 `SearchCtrl` 인스턴스가 생성되는게 아닐까? 이때 생성자(function) 에 파라미터로 선언한 인자들을 Angular 가 Guice 처럼 DI 해주는거고?

## ECMAScript? TypeScript?

[관련 포스트 링크](http://learnangular2.com/es6/)

**ECMA** 는 재단 이름이다. 여기서 발표하는 JavsScript 표준이 ECMAScript 이다. 새로운 ECMAScript 가 발표되면, 브라우저 벤더들이 이에 맞춰 구현하며 새로운 브라우저 버전을 준비하는데 그 사이에 시간 간격이 생긴다. 개발자들은 항상 최신의 ECMAScript 를 사용하고 싶어 하므로, **Babel** 같은 컴파일 도구를 활용해서 최신 ECMAScript 로 작성된 코드를 현재 운용중인 브라우저가 사용하는 ECMAScript 버전에 맞춰 컴파일하면 된다. 아마 현재는 ECMAScript 2015 = ES6 가 웬만한 최신 브라우저의 표준인듯 하다. 그 예로 크롬 개발자 도구 console 에서 화살표 함수가 정상적으로 실행된다.

**TypeScript** 는 MS 에서 만든 JavsScript 의 high-level 언어로, 그 도입 배경은 Babel 을 사용하는 이유와 유사하다. 마찬가지로 자체적인 컴파일러를 제공한다. Angular 2 나 Ionic 2 가 TypeScript 을 사용했으므로 이들 라이브러리(?)를 사용하거나 contribute 하고싶은 경우 TypeScript 를 배우면 좋을 것이다.
****
## 모듈 로더와 모듈 번들러

[CommonJS 홈페이지](http://www.commonjs.org/)
[Naver D3 Hello world 블로그의 "JavaScript 표준을 위한 움직임: CommonJS 와 AMD"](http://d2.naver.com/helloworld/12864)
["webpack 실전 가이드" 라는 포스트](https://hyunseob.github.io/2016/04/03/webpack-practical-guide/)

**CommonJS** 는 여러 분야에서 활용되는 JS 들의 standard 를 정의하기 위한 시도이다. 마치 Python, Ruby 같은 언어처럼 CommonJS API 가 방대한 built-in 라이브러리 역할을 한다. 개발자가 CommonJS API 로 개발하면, 여러 JS 인터프리터에서 이를 실행할 수 있다. 이러한 CommonJS API 에서는 **import** 를 제공하고, 이를 따르는 node.js 에서도 import 가 가능하다. 그러나 브라우저가 사용하는 JS 인터프리터에서는 import 를 제공하지 않는다.

그래서 브라우저를 위해 CommonJS 의 import 스펙을 구현한 라이브러리들이 나왔다. **RequireJS** 나 **AMD** 같은 모듈 로더들이 대표적이다. 모듈 로더들은 동적 스크립트 로딩을 통해 import 문제를 해결한다.

이에 반해 **Webpack** 은 모듈 번들러로써 이 문제를 해결한다. import 가 필요한 상호 의존적인 스크립트들을 한데 묶어서 번들로 만들고,
그 번들을 브라우저가 로딩한다.