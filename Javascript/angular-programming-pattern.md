## AngularJS 프로그래밍 패턴에 대한 고민
#### 배경
* jQuery 로 프로그래밍 한뒤에 AngularJS 로 치환하는 과정은 어렵지 않다. jQuery 에서 selector 목적으로 달아놨던 id/class 들을 제거하고, **ng-model**(데이터 바인딩), **ng-click**(클릭 이벤트 리스너), **ng-show**(display 제어), **ng-repeat**(template engine 의 for loop 대체) 등을 적절히 사용하면 깔끔하게 대체할 수 있다.
* 하지만 이게 AngularJS 의 전부는 아닐거라는 생각이 계속 들고, 아마 확실할 것이다. AngularJS 의 강력한 무기인 **양방향 데이터 바인딩**과 **$scope** 계층 구조를 활용해서 적절히 **모듈화된 DOM 구조**를 만들 수 있지 않을까 하는 추측이다.

#### 첫번째 시도: 단일 app 구조
* 메인 HTML 페이지가 있다. 서비스의 진입 URL 에서 이 페이지를 반환한다. 서비스에서 필요한 모든 CSS/JS 는 메인 페이지의 `<head>` 에 선언된다. 혹은 모든 페이지들이 공통으로 필요한 CSS/JS 들만 메인 페이지에 선언하고, 각자 필요한 CSS/JS 들은 각각의 페이지에서 로딩하도록 나눌 수도 있다. 
* 메인 HTML 페이지의 `<body>` 에 단일 app 이 연결된다.
* 서비스에서 필요한 모든 HTML 페이지(component)들을 종류별로 단일 app 의 디렉티브로 선언한다. 가장 간단하게는 디렉티브의 팩토리에서 **templateUrl** 값만 설정하면 된다. 이를 위해서 templateUrl 값에 대응하는 HTML 을 반환하는 백엔드가 필요하다. 디렉티브마다 **ng-show** 를 달아놓고 이를 위한 boolean 변수를 단일 app 의 **$scope** 에 선언해서 visibility 를 제어한다.
* 디렉티브로 선언된 element 들의 데이터 바인딩이 필요하다면, 모두 모아서 단일 app 의 **$scope** 에 바인딩한다. 디렉티브들간의 변수명이 충돌하지 않도록 적절한 네임 스페이스를 구성한다.

```html
<!--메인 페이지-->
<body ng-controller="mainController">
    <!--각각의 디렉티브들의 가시성 변화를 트리거하는 컴포넌트들도 필요하다.-->
    <button ng-click="clickOne()">페이지1</button>
    <button ng-click="clickTwo()">페이지2</button>
    <!--디렉티브들-->
    <div my-page-one ng-show="pageOneVisible"></div>
    <div my-page-two ng-show="pageTwoVisible"></div>
</body>
```
```html
<!--my-page-one 디렉티브를 위한 template, my-page-two 도 거의 동일하다.-->
<h1>page1.html 입니다</h1>
<!--각각의 template 에서 interpolation 될 변수들은 단일 app 으로 모두 모일것이다.-->
<p>page1 의 var1: [[ var1_1 ]]</p>
<p>page1 의 var2: [[ var1_2 ]]</p>
```
```javascript
app.controller("mainController", function($scope) {
    $scope.pageOneVisible = false;
    $scope.pageTwoVisible = false;

    $scope.clickOne = function() {
        $scope.pageOneVisible = true;
        $scope.pageTwoVisible = false;
    };
    $scope.clickTwo = function() {
        $scope.pageOneVisible = false;
        $scope.pageTwoVisible = true;
    };

    // 한곳에 모두 모인 변수들!
    $scope.var1_1 = "one-one";
    $scope.var1_2 = "one-two";
    $scope.var2_1 = "two-one";
    $scope.var2_2 = "two-two";
});

// templateUrl 값으로 들어가는 URL 에 맞춰 HTML 을 서빙하는 백엔드가 요구된다.
app.directive("myPageOne", function() {
    return {
        templateUrl: "frontend-page1"
    };
});
app.directive("myPageTwo", function() {
    return {
        templateUrl: "frontend-page2"
    };
});
```
* 디렉티브들을 모두 로딩해놓고, visibility 만 제어하기 때문에 비효율적이라는 생각이 들었다. visibility 를 제어하는 차원이 아니라, 아예 HTML 을 갈아끼우는 방식의 구현이 더 낫지 않을까 싶었다.
* 이와 관련해서 혹시 **ng-include** 나 **ng-view** 디렉티브가 큰 도움이 되지 않을까 생각했다.