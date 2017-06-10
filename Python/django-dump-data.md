[SQLite3의 데이터를 PostgreSQL로 이전하기](http://technerd.tistory.com/58)

### Django 커맨드 + 설정파일로 DB 데이터 dump

```shell
python manage.py dumpdata --natural-primary --natural-foreign --exclude=contenttypes --settings winning.settings_prod > migration.json
```

`winning.setings_prod` 는 사용할 Django 설정파일 경로, `migration.json` 은 dump 파일 경로를 명시한다.

### EC2 머신 -> 로컬로 파일 복사

```shell
scp -i "<PEM 파일 경로>" ubuntu@52.78.49.143:/home/ubuntu/Projects/winning/migration.json migration.json
```

첫번째 인자는 EC2 머신의 파일 경로, 두번째 인자는 로컬 파일 경로를 명시한다. 위 커맨드는 로컬 머신에서 실행한다.
