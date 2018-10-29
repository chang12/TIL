# .gitignore_global

IntelliJ 로 open 한 project 에 csv 파일을 추가하니, csv plugin 을 설치하라는 안내 문구가 떴다. 그래서 설치했다. 그 이후로 csv 파일을 추가해도 sourcetree 가 track 하지 못했다. project 의 `.gitignore` 에는 새로 패턴이 추가된게 없었다.

* `git status --ignored` 를 실행하니 `*.csv` 패턴이 ignore 대상이다.
* `git check-ignore -v temp.csv` 를 실행하니 `*.csv` 패턴이 `.gitignore_global` 이라는 파일에 적혀있다.

home directory 를 확인해보니 .gitignore_global 이라는 파일이 존재한다.

# .gitconfig

Sourcetree 로 커밋 히스토리를 보면 "작성자" 탭이 있는데 거기에 대학교 메일과 한국 이름의 영문명이 적혀있었다. 회사 메일과 회사 영어 이름으로 바꾸고 싶어서 찾아보니, `~/.gitconfig` 에 적힌 `user.name` 과 `user.email` 을 바꾸면 되는거였다. 바꾸고나니 이후 커밋부터 바로 반영되었다.

# amend

커밋 후에 미처 커밋에 포함시키지 못한 자잘한 수정이 떠오르는 경우들이 있다. 이때 commit 의 amend 옵션을 사용하면 편리하게 해결할 수 있다. [Git Basics: Adding more changes to your last commit](https://medium.com/@igor_marques/git-basics-adding-more-changes-to-your-last-commit-1629344cb9a8) 포스트에서 처음 접했다. Sourcetree 에서는 커밋할때 커밋 옵션을 설정할 수 있는데, `마지막 커밋 수정` 을 선택하면 된다.

# GitHub Deploy Keys

ubuntu EC2 머신에서 private GitHub repo 를 clone 받기 위해 ssh key 를 만들고, Deploy Keys 에 등록한다. `git clone git@github.com:{organization}/{repo}.git` 으로 clone 을 받는데, 이때 ssh config 를 설정해둬야 한다.

```ssh
Host github.com
    HostName github.com
    User git
    IdentityFile {path_to_ssh_private_key}
```