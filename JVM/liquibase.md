몇일전에 Python 진영의 `alembic` 패키지의 기본 사용법을 익혔었는데, Java 진영에서 이에 대응하는게 `Liquibase` 라는 얘기를 회사에서 들었던게 생각나서 아주 간단하게 다뤄봤다. [Liquibase 공식 홈페이지](http://www.liquibase.org/) 에 접속하면 **"SOURCE CONTROL FOR YOUR DATABASE"** 라고 자신을 소개한다.

### Download

[다운로드 페이지](http://www.liquibase.org/download/index.html) 에서 압축 파일을 다운로드하고, 압축을 해제한다. Java 공부용으로 사용하는 study-java 모듈에 복붙했다. 바이너리로 사용하지 않고, Maven plugin 으로 사용하는 방법도 있다고 하는데, 빌드 과정에서 말고 일단 빠르게 한번 돌려보고 싶었으므로 바이너리를 사용했다.

### Change Log 파일 작성

[QUICK START](http://www.liquibase.org/quickstart.html) 문서를 보고 내용을 그대로 따라 Change Log XML 파일을 작성했다. 하지만 메인 페이지를 보니 **YAML / JSON / SQL 포맷으로도** 작성할 수 있었다.

### Command 실행

위의 QUICK START 문서의 Step 3 항목에 적힌 커맨드를 사용한다. `password` 는 중간에 `=` 이 들어가있으므로 "..." 로 감싸줬다. `classpath` 는 JDBC Driver 클래스가 포함된 JAR 파일로 지정했다. 실행하기전에 HeidiSQL 로 접속해서 study-liquibase 라는 이름으로 데이터베이스를 새로 만들었다.

```
--classpath=C:\Users\User\.m2\repository\mysql\mysql-connector-java\6.0.5\mysql-connector-java-6.0.5.jar
```

여기까지 진행하고 HeidiSQL 로 확인해보니 정상적으로 테이블이 생성되었다. `alembic` 이 그러하였듯이, `Liquibase` 도 마찬가지로 버전 정보등의 메타데이터를 저장하는 테이블을 추가로 생헝한다.
