## 목록
* [유틸리티 메서드는 **factory recipe** 로 구현하자.]()

## 유틸리티 메서드는 **factory recipe** 로 구현하자.
유틸리티 메서드를 module 에 factory recipe 로 구현해놓으면, 다른 controller 에서 편리하게 주입받아 사용할 수 있다.

```javascript
.factory("utilityFunctions", function utilityFunctionsFactory($location) {
    // factory recipe 도 $location 등을 주입받을 수 있다. 
    return {
        "routeTo": function(address) {
            $location.path('/' + address);
        }
    };
})
```
```javascript
.controller("MainController", function($scope, utilityFunctions) {
    // 필요한 유틸리티 메서드만 선택해서 scope 에 바인딩
    $scope.routeTo = utilityFunctions.routeTo;
})
```

만약 backend 와 함께 동작하는 웹앱이라면, $http 를 사용해서 backend API 에 HTTP 요청을 보내는 코드들을 한곳에 모아서 factory recipe 로 만들고, controller 에서는 뽑아서 사용하면 어떨까 생각했다. **"backend API 를 때리는 로직"** 을 한데 모아둘 수 있으므로 유지보수가 편할거라는 생각이었다.