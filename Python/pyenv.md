## pyenv
community-server 를 세팅하려던 맥락이었다. 서버별로 python 2/3 여부가 갈리기 때문에 좀 편리하게 관리하고 싶었다.

https://github.com/yyuu/pyenv
https://github.com/yyuu/pyenv-virtualenv
https://github.com/yyuu/pyenv-virtualenvwrapper

* `brew install pyenv` 로 pyenv 를 설치한다.
* ~/.bash_profile 에 eval 커맨드를 추가한다.
* `pyenv install 3.5.2` 로 3.5.2 버전을 다운로드 한다.
* `brew install pyenv-virtualenv` 로 pyenv-virtualenv 를 설치한다.
* ~/.bash_profile 에 eval 커맨드를 추가한다.
* `pyenv virtualenv 3.5.2 community-server` 로 3.5.2 버전에 virtualenv 를 추가한다.
* PyCharm 의 Project Interpreter 설정에서 ~/.pyenv/versions/community-server 디렉토리의 interpreter 를 지정한다.

핵심은 virtualenv 를 python version 별로 편리하게 만들 수 있다는 점이다. 만들어진 virtualenv 의 python interpreter 를 PyCharm 에 지정하는 것은 동일하다.