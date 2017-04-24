## MySQL 에서 user 테이블 레코드의 password 변경

[MySQL 문서의 SET PASSWORD Syntax 문서를 참고해서 패스워드를 설정할 수 있었다](https://dev.mysql.com/doc/refman/5.7/en/set-password.html)

### RDS

```mysql
SET PASSWORD FOR chang12 = "password"
```

### 로컬 MySQL

로컬 머신에서는 `host` 를 입력하지 않으니 SQL 문이 에러를 발생시킨다

```mysql
SET PASSWORD FOR root@localhost = "password"
```
