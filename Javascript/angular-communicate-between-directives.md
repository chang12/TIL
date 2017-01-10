하나의 HTML 파일을 여러개의 컴포넌트가 구성한다. 컴포넌트를 나누는 기준은 현재까지는 두 가지다. 개념적으로 분리되거나(ex: query 를 날리기 위한 검색어를 받는 input + button 과, API 호출 후 상태를 나타내는 alert 는 개념적으로 다르므로 분리한다.), 다른 XHR 응답에 영향을 받는 단위로 나누고 있다. 현재 기준에서 나눠진 컴포넌트들은 100% 독립적으로만 동작할 수는 없다. 그러므로 이해하기 쉬운 형태로 서로 상호작용하게 코딩하고 싶었다. 

## component 의 require 를 사용 (실패)
처음에는 component 의 **require** 옵션으로 쉽게 해결할 수 있어보였다. 하지만 require 옵션은 상호 포함관계에서 위/아래 방향으로만 적용 가능하고, sibling 관계에는 적용할 수 없었다. 그러므로 포기.

## directive 의 link 를 사용 (성공)
임시방편으로 directive 의 **link 옵션**을 사용할 수 있었다. component 는 link 옵션이 없으므로, directive 를 사용한다. 

우선 directive 들을 감싸는 **parent controller** 를 정의하고, scope 에 **빈 dictionary** 를 선언한다. 이는 자식 directive 들이 서로 공유하는 일종의 namespace 역할을 맡는다.

```javascript
angular
    .module("모듈_이름")
    .controller("DummyCtrl", ["$scope", function($scope){
        $scope.share = {}; // 빈 dictionary
    }])
;
``` 

위에서 선언한 빈 dictionary 변수를, HTML 코드에서 자식 directive 들에게 주입한다. directive 들은 **'='** 키워드를 사용해서 주입받은 변수를 자신의 **isolated scope** 에 바인딩할 수 있다.

```html
<div ng-controller="DummyCtrl">
    <child-one-directive share="share"></child-one-directive>
    <child-two-directive share="share"></child-two-directive>
</div>
``` 

이제 directive 를 선언할때, 다른 directive 에서 호출할 수 있는 함수들을 주입받은 dictionary 에 entry 로 추가한다. directive 의 link 옵션에서 가능하다. link 옵션에서 scope 를 파라미터로 받는 덕분이다.

```javascript
angular
    .module("모듈_이름")
    .directive("childOneDirective", function() {
        return {
            scope: {
                share: '='
            },
            link: function(scope) {
                scope.share.functionToBeCalledBySiblings = function() {
                    // 로직 기술
                };
            },
            ...
        }
    })
;
```

어떤 함수들이 공유될지 한눈에 알 수 있도록, 미리 controller 의 dictionary 에 **undefined** 로 선언하는 방법도 가능하겠다. 물론 Javascript 는 정적 언어가 아니기 때문에, 이를 강제할 방법은 없을 것.
