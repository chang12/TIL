### HTML reserverd characters / character entities

HTML 의 **reserved characters** 들을 본래 목적외에 순수한 문자열로 사용하고싶다면, **character entiies** 를 사용해야한다. HTML 문서를 구성하는 핵심 요소인 `<tag>` 구문에 사용되는  `<`, `>` 를 예로 들 수 있다. 만약 HTML 작성자가 HTML 태그를 적으려는 의도가 아니라 `<tag>` 라는 문자열을 적고싶은 상황을 가정해보자. `<`, `>` 기호를 그대로 사용하면 브라우저는 이를 태그로 인식할 거이다. 그러므로 character entities 를 사용해서 `&lt;tag&gt;` 처럼 적어줘야한다. 

character entities 는 `entity_name` 과 `entity_number` 의 2가지 표현이 가능하다. 앞선 경우는 `entity_name` 을 사용한 경우고, `&#60;tag&#62` 처럼 적어서 `entity_number` 로 사용할 수도 있다. 사용가능한 character entities 들에 대해서는 [HTML Entity List](http://www.freeformatter.com/html-entities.html) 링크에 잘 정리되어있다.

그러므로 Django 에서 템플릿 기능을 사용하거나, Flask 로 개발하면서 Jinja2 같은 템플릿 엔진을 사용해서 HTML 페이지의 내용을 동적으로 렌더링할 경우 템플릿 엔진이 알아서 변환해줘야 한다. 예를 들어 `<h1>{{ name }}</h1>` 같은 HTML 페이지가 있고, 코드에 선언된 name 변수의 값이 `"<Max>"` 일 수 있다. 이를 그대로 사용하면 안되고, `"&lt;Max&gt;"` 로 변환해서 렌더링해주면 브라우저에 보일때는 h1 으로 강조된 `"<Max>"` 를 보게 된다.

### Javascript 렌더링

위 문단에서 얘기한 템플릿 엔진의 렌더링 기능을 HTML 페이지에 `<script>` 로 삽입된 JS 코드에도 적용할 수 있다. 예를 들어 브라우저 콘솔에 로깅하는 문자열을 렌더링하는 상황을 생각해보자.

```html
<body>
    ...
    <script type="text/javascript">
        console.log("{{ JS_VAR }}");
    </script>
</body>
```

이때 렌더링되는 문자열 변수에, `<Text>` 처럼 HTML 의 reserved characters 가 포함될 수 있다. 위 문단에서 기술한 템플릿 엔진의 HTML escape 과정을 거쳐 `&lt;Text&gt;` 로 변환될 것이다. 그러나 브라우저가 HTML 페이지를 처리할때는 escape 된 character 들을 다시 원복시켜서 보여주지만, Javascript 인터프리터는 그러지 못한다. 위 코드 블록에서 `JS_VAR` 값이 `<Text>` 였다면 콘솔에 찍히는 문자열은 `&lt;Text&gt;` 가 되는 것이다.

이를 해결하기 위해서 Django 프레임워크에서는 `escape_js` 라는 기본 템플릿 필터를 제공한다. [GitHub 코드를](https://github.com/django/django/blob/master/django/utils/html.py#L71) 확인해보면 HTML reserved characters 들에 대해서 character 를 변환해주는 역할의 필터다. 즉, Javascrip 인터프리터 구성에 따라서 `<` 는 `\u003C` 로, `>` 는 `\u003E` 로 변환해서 `<Text>` 로 정상적으로 렌더링되게 만든 것이다.

Jinja2 에서는 이러한 기본 필터를 제공해주지 않아서 아쉬운 상황이다. 현재까지 파악한 가장 좋은 방법은, Django 의 `escape_js` 필터 구현 코드를 참고해서 커스텀 필터를 만들어 Flask 의 Jinja2 환경에 추가해서 사용하는 것이다.
