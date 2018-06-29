## Install / Uninstall

* [Download MySQL Community Server](https://dev.mysql.com/downloads/mysql/) 에서 5.7.22 버전 dmg 파일을 다운로드받아 설치 프로그램을 실행한다.
* 설치중에 root 의 임시 password 를 발급하고 알려주길래 적어놨다.
* terminal 에서 mysql 프로그램을 실행하고 `mysql -u root -p` 명령어로 root 로 접속한다. 적어놓은 임시 password 를 입력한다.
* `alter user 'root'@'localhost' identified by 'new_password'` 명령어로 root 의 password 를 변경한다.
* 삭제할때는 [Uninstall MySql on a Mac OS X](https://community.jaspersoft.com/wiki/uninstall-mysql-mac-os-x) 를 참고했다.

## DDL

* `alter table {tbl_name} add column {col_name} {data_type} after {other_col_name}`
* `create view {view_name} as select ...`

## DML

### insert insert ... on duplicate key update ...

[MySQL update if value is greater than that current value 답변](https://stackoverflow.com/a/10081527)

```sql
insert into monthlystats (id, server, time, uptime, players, rank) 
values (09126, 6, 0912, 302, 0, 1) 
on duplicate key update 
    uptime = greatest(uptime, values(uptime)), 
    players = greatest(players, values(players)),
    rank = greatest(rank, values(rank))
```

`greatest`, `least` function 의 parameter 중 하나라도 null 이면 null 을 반환하기 때문에, nullable column 을 다룰경우 `coalesce` 로 null 인 경우를 고려해줘야한다.

## Functions

* `cast("2017-11-30 00:14:42" as date)`
* `convert_tz("2017-11-30 00:14:42", "UTC", "Asia/Seoul")`

## Shell

mysql shell 에서 command 뒤에 `\G` 를 붙이면 더 보기좋게 출력해준다.

## Permission

table 단위로 조회를 허용할 수 있었다.

```sql
grant select, show view 
on {database_name}.`{table_name}` 
to '{user_name}'@'%'
```

