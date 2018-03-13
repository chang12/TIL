# `.gitignore_global`

IntelliJ 로 open 한 project 에 csv 파일을 추가하니, csv plugin 을 설치하라는 안내 문구가 떴다. 그래서 설치했다. 그 이후로 csv 파일을 추가해도 sourcetree 가 track 하지 못했다. project 의 `.gitignore` 에는 새로 패턴이 추가된게 없었다.

* `git status --ignored` 를 실행하니 `*.csv` 패턴이 ignore 대상이다.
* `git check-ignore -v temp.csv` 를 실행하니 `*.csv` 패턴이 `.gitignore_global` 이라는 파일에 적혀있다.

home directory 를 확인해보니 .gitignore_global 이라는 파일이 존재한다.