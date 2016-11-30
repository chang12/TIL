프런트엔드 라이브러리 중 유일하게 알고있는 것이 **jQuery** 이다. 백엔드 프레임워크 중에서도 유일하게 사용하는 것이 **Django** 이므로 그 사용 패턴이 거의 동일하다.

* Django 의 템플릿 렌더링으로 HTML 파일을 만든다.
* jQuery 가 사용된 스크립트가 HTML 파일에 붙는다.
* jQuery 로 HTML 파일의 form 데이터를 묶고, 직렬화한다. 
* 직렬화한 데이터를 AJAX GET/POST 메서드로 Django view 에 쏜다.
* Django view 가 작업을 이어나간다.

이 중에서 세번째 작업이 그때그때 코드가 조금씩 달라지는 것 같아서 스트레스였는데, 오늘 회사에서 마음먹고 한번 정리해보려다가 일단락 된 듯 하여 정리해본다.

## 사전 준비
* Django view 에 값(value)이 전달되기를 원하는 모든 DOM element 에 **name 속성값을 추가**한다. name 값이 곧 key 값이 된다.
* 함께 직렬화되기를 원하는 element 들을 모아서 하나의 태그 아래에 배치한다. selector 가 해당 태그를 적절히 식별할 수 있도록 준비한다.

## 직렬화
name 속성값이 있는 element 들만 선택하는 selector 를 사용한다. [[링크]](https://api.jquery.com/has-attribute-selector/)
```javascript
var data = $("...").find("[name]").serialize(); // "key1=value1&key2=value2&..."
var data = $("...").find("[name]").serializeArray(); // [Object, Object, Object]
```