`Flask` 로 개발할때 HTML 페이지가 수정되면 자동으로 즉각 이를 반영해주는 기능이 있다. 따로 설정하지 않으면 성능을 위해서 HTML 페이지를 캐싱해두는 듯 하다. `Flask` 인스턴스를 만들고 설정 메서드를 호출하면 된다.

```python
app = Flask()
app.config['TEMPLATES_AUTO_RELOAD'] = True
```

그러던 중 어느날 갑자기 이 자동 로딩 기능이 동작하지 않아서 애먹었던 적이 있었다. 찾아보니 원인은 `Jinja` 템플릿 태그를 등록하는 코드와 조합이 안맞아서였다.

```python
# 이 경우에는 정상 동작한다. HTML 파일을 수정하면 update 된다.
app.config['TEMPLATES_AUTO_RELOAD'] = True
app.jinja_env.filters['escapejs'] = escapejs

# 그런데 코드 순서를 바꾸면 문제가 생긴다. HTML 파일을 수정해도 app 자체가 reload 되기전까지 update 되지 않는다.
app.jinja_env.filters['escapejs'] = escapejs
app.config['TEMPLATES_AUTO_RELOAD'] = True 
```

찾아보니 Flask GitHub 에서도 얘기되는 주제였다. [Jinja templates do not auto reload if Flask app debug enabled solely via app.run(debug-True)](https://github.com/pallets/flask/issues/1907) issue 를 발견할 수 있었고, 원인을 이해할 수 있었다. jinja environment 객체의 auto reload 설정은 Flask 의 설정값을 따라가고, jinja environment 는 jinja_env 객체가 접근될때 instantiate 된다. 그러므로 코드 순서에 따라서 jinja environment 의 auto reload 설정값이 달라지는 문제였다.

issue 를 제안한 사람이 pull request 까지 만들어서 지난 1월에 요청했지만 아직까지 Open 상태이다. 아마 Flask 단을 수정하지 않고도, 조금 신경써주면 크게 무리하지 않는 선에서 work around 할 수 있기 때문인가보다. 