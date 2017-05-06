기존에 Django 로 개발했던 간단한 웹 어플리케이션을 Flask 로 포팅하고있다. 회사에서 Flask 를 주로 사용하므로 그와 관련된 개발 스택을 익히기 위함이다. 기존에 Django ORM 을 사용하던 부분을 대체하면서 SQLAlchemy 도 사용하게 되었다.

## engine 생성

SQLAlchemy 로 DB 를 조작하기 위해서는 DB 서버와 통신을 규정하는 `Engine` 인스턴스를 생성해야 한다. `create_engine` 메서드로 생성할 수 있다. AWS RDS 로 띄운 MySQL DB 머신의 endpoint 값에 username / password 가 적혀있으므로 이 값만 넘겨서 `Engine` 을 만들 수 있었다.

## session 생성

SQL 을 통한 조작은 `Session` 인스턴스를 통해서 이뤄진다. 우선 `sessionmaker` 메서드로 `Session` 클래스를 정의한다. 이때 engine 인스턴스가 사용된다. `Session` 클래스를 선언하고나서는 빈 생성자로 session 인스턴스를 만들 수 있었다.

## add / commit

DB 테이블에 mapping 되는 클래스의 인스턴스를 만들고, `session.add()` 메서드를 호출할 수 있다. 아직까지는 persist 된 상태는 아니다. `session.commit()` 메서드를 호출하면 Transaction 하에서 INSERT 쿼리가 실행된다. HeidiSQL 로 확인해보면 정상적으로 user 테이블에 record 가 생성되었다.
