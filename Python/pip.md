현재 설치된 패키지들의 리스트를 txt 파일로 생성한다.
```
pip freeze > requirements.txt
```
생성한 txt 파일로 다른 환경에서 프로젝트에 필요한 패키지들을 간편하게 설치할 수 있다.
```
pip install -r requirements.txt
``` 