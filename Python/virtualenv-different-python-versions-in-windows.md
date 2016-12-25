Unix 계열의 OS 라면 [pyenv](https://github.com/yyuu/pyenv) 와 그 시리즈들을 사용해서 편리하게 다양한 Python 버전별로 virtualenv 를 구성할 수 있다. 하지만 Windows 의 경우 pyenv 를 사용할 수가 없는데, 이를 대체할만한 그 무언가를 찾는게 어려웠다.

그래서 생각해낸 꼼수가 **PyCharm** 을 활용하는 것이다. PyCharm 의 Settings 메뉴에서 새로운 virtualenv 를 생성할 수 있는데, 이때 Base interpreter 를 선택한다. 이때 이 Base interpreter 를 원하는 버전의 Python 의 interpreter 파일로 선택하면 된다.