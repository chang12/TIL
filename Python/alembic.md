[Alembic Tutorial](http://alembic.zzzcomputing.com/en/latest/tutorial.html#partial-revision-identifiers) 문서를 따라가면서 DDL 관련 작업들을 수행하는 기본적인 방법들을 익힐 수 있었다.

## 환경 준비

* `pip install alembic`
* `pip install mysqlclient` -> MySQL Python connector 사용을 위해서 필요함

## alembic.ini 파일 수정

* `alembic init 디렉토리_이름` 명령어로 alembic 디렉토리 구조를 생성한다.
* 명령어를 생성한 경로에 `alembic.ini` 파일이 만들어진다.
* `sqlalchemy.url` 값을 사용할 DB 머신 + 데이터베이스에 대한 값으로 변경한다.

## migration script 파일 작성
* `alembic revision -m "migration 스크립트 제목"` 명령어로 migration script 파일을 생성한다.
* 위에서 만든 alembic 디렉토리 하위의 versions 디렉토리 밑에 script 파일이 만들어진다. 고유 식별자 + 명령어에 넣은 스크립트 제목으로 파일명이 정해진다.
* `revision` 은 자신의 식별자, `down_revision` 은 이전 migration 의 식별자이다.
* `upgrade` / `downgrade` 함수를 작성한다. alembic 이 제공하는 directive 들을 활용한다.

## upgrade / downgrade

* `alembic upgrade head` -> 가장 최신까지 migration 을 실행한다.
* `alembic downgrade base` -> 모든 migration 을 roll-back 한다.