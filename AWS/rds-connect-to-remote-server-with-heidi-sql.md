시작은 **SQLAlchemy** 를 좀 공부해보려던거였다. 로컬에서 안하고 원격 DB 에 대해서 하고싶은 마음이 피어올라서 MySQL AWS RDS DB 인스턴스를 만들었다. 로컬 머신 (윈도우 노트북) 에서 접속하기 위해 몇가지 단계를 거쳐야 했다.

## MySQL 클라이언트 프로그램 설치

예전에 컴공 데이터베이스 수업 프로젝트때 생각이 나서 [HeidiSQL](https://www.heidisql.com/) 을 설치했다. MySQL 에서 제공하는 WorkBench 라는 프로그램도 있는것 같다.

## Security Group 설정

RDS DB 인스턴스 만들때 기존에 있는 security group 을 사용했다. RDS 콘솔에 접속해서 DB 인스턴스를 찍어보면 사용한 Security Group 으로 연결된 링크를 확인할 수 있다. **MySQL/Aurora** 항목으로 **3306 포트**에 **Anywhere** 로 새로운 Rule 을 추가해준다.

## HeidiSQL 로 접속

처음에 security group 에서 rule 추가하지 않고 접속하려다가 자꾸 실패해서 방황했었다. SSH 터널링이 필요한가 싶어 PuTTY 로 pem 파일을 ppk 파일로 변환하는 등의 방황의 시간을 좀 보냈었다. security group 에 rule 추가하고나면, **host** / **username** / **password** 만 잘 입력해주면 된다. username / password 는 DB 인스턴스 만들때 입력해준 값이다.