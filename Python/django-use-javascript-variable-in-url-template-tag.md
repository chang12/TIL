[stackoverflow 링크](http://stackoverflow.com/questions/17832194/get-javascript-variables-value-in-django-url-template-tag)

```javascript
var id = ...
var url = "{% url 'post:erase' 12345 %}".replace(/12345/, id);
```

## 상황
- 메인 화면에 리스트 아이템들이 있고, 각각에는 체크박스가 달려있다. 
- 체크박스가 체크되면, 해당 리스트 아이템의 삭제를 요청하는 클릭 이벤트 핸들러를 달아주고 싶었다. 
- HTML 페이지의 리스트 아이템은 자신의 모델 인스턴스 pk 값을 id로 가지고 있어서 그 값을 url 태그에 사용하면 될 것 같았다.
- 하지만 `{% url 'post:erase' id %}` 처럼 활용은 불가능했다. Django의 템플릿 태그에서 해당 템플릿의 스코프에 정의된 Javscript 변수까지 읽어들이지는 못하나보다.

구글링해서 위와 같은 임시방편 해결책은 찾았는데, Django 차원에서 제공해주는 더 깔끔한 방법은 없을까... 싶었다.