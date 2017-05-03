Django 를 AngularJS 와 결합하려고 시도중이다. AngularJS 를 비롯한 각종 프런트엔드 라이브러리들을 npm 을 통해서 다운로드 받는다. 그러고 template HTML 에서 CSS/JS 파일들에 링크를 걸어줘야 하는데, 이를 위해서는 Django 백엔드에서 파일들을 서빙해줘야한다.

## Basic
언제나 그렇듯이, [Django document](https://docs.djangoproject.com/en/1.10/howto/static-files/#managing-static-files-e-g-images-javascript-css) 를 참고한다.

* settings.py 에 `STATICFILES_DIRS` 리스트를 설정한다. Django 의 정적 파일 파인더가 탐색할 경로를 추가하는 것. `BASE_DIR` 경로에 대한 상대경로로 `node_modules` 디렉토리를 추가했다.
* template HTML 에 `{% load static %}` 구문을 추가한다. `static` template tag 를 사용할 수 있게된다.
* href / src 속성값을 적을때 해당하는 `STATICFILES_DIRS` 의 원소에 대한 상대경로로 기술한다.

settings.py 에 바로 드러나지 않지만 중요한 이슈 한가지는 app 디렉토리에 static 서브 디렉토리가 존재한다면 정적 파일 파인더가 기본적으로 탐색해준다는 점이다. 편리하다. 다만 네이밍에 유의해서 파일명이 겹치지 않도록 한다. 예를 들어 모든 app 마다 index.js 같은 이름은 존재할법 하므로 static 서브 디렉토리 아래에 app_이름/index.js 같이 app_이름을 한번 더 써줘서 구분짓는다. 