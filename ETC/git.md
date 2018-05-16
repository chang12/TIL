# .gitignore_global

IntelliJ 로 open 한 project 에 csv 파일을 추가하니, csv plugin 을 설치하라는 안내 문구가 떴다. 그래서 설치했다. 그 이후로 csv 파일을 추가해도 sourcetree 가 track 하지 못했다. project 의 `.gitignore` 에는 새로 패턴이 추가된게 없었다.

* `git status --ignored` 를 실행하니 `*.csv` 패턴이 ignore 대상이다.
* `git check-ignore -v temp.csv` 를 실행하니 `*.csv` 패턴이 `.gitignore_global` 이라는 파일에 적혀있다.

home directory 를 확인해보니 .gitignore_global 이라는 파일이 존재한다.

# .gitconfig

Sourcetree 로 커밋 히스토리를 보면 "작성자" 탭이 있는데 거기에 대학교 메일과 한국 이름의 영문명이 적혀있었다. 회사 메일과 회사 영어 이름으로 바꾸고 싶어서 찾아보니, `~/.gitconfig` 에 적힌 `user.name` 과 `user.email` 을 바꾸면 되는거였다. 바꾸고나니 이후 커밋부터 바로 반영되었다.