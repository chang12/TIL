## 구글링
http://codeutopia.net/blog/2013/05/27/3-ways-to-get-backend-data-to-angularjs/
module.value 를 활용할 수 있다. value 선언 후에 controller 선언할때 이름만 맞춰주면  Angular DI 가 알아서 주입해준다.
```javascript
var myModule = angular.module('example', []);

myModule.value('stuff', { hello: 'foobar' });

myModule.controller('FooCtrl', function($scope, stuff){
    // 'stuff' 를 Angular DI system 이 주입해준다.
});
```
근데 backend 로부터 데이터를 받아오려는 상황에는 부적절해보인다. **JS 파일을 backend 에서 렌더링하지 않을 것** 이기 때문이다. 다만 같은 app 의 여러 controller 들 사이에 데이터를 공유하기에는 적절해보인다.
## from Ted
script 태그 안에서 server-side template engine 의 도움을 받아 backend 의 데이터로 초기화한다.
```html
<script>
    SOME_VARIABLE = {{ jinja_interpolation | json | safe }};
</script>
```