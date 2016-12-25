## 참고문헌
- [Wikipedia](https://en.wikipedia.org/wiki/Isolation_(database_systems))

## Terminology
### Dirty Read
다른 transaction(T2) 의 commit 되지 않은 update 가 반영된 데이터를 읽는 것. 그러므로 T2 가 이후에 roll-back 되거나, 또 다른 update 를 가하는 경우 내가 읽은 데이터의 consistency 가 깨진다. 
### Non-repeatable Read
데이터를 한번 읽고(Q1), 다른 transaction(T2) 이 그 데이터를 update 후 commit 하고, 다시 읽었을 때(Q2) Q1 과 달라지는 것. Q1, Q2 가 같은 transaction 에 있더라도 같은 데이터의 다른 값을 읽게된다.
### Phantom Read
`SELECT ... WHERE` 류의 range scan 에서의 얘기다. 데이터를 한번 읽고(Q1), 다른 transaction 이 Q1 의 WHERE 조건을 만족하는 새로운 데이터를 insert 하고, 다시 읽었을때(Q2) 새로 insert 된 row 까지 읽어서 Q1과 Q2의 결과가 달라지는 것을 의미한다.

## Isolation Level
isolation level 의 개념은 **ANSI/ISO SQL standard** 로 규정되어있다. isolation level 이 높을수록 consistency 는 더 강하게 보장되고, performance(concurrency level) 는 떨어진다.
### Serializable
가장 높은 isolation level 로, serial schedule 과 동일한 결과를 내는 serializable schedule 만 허용하는 level 이다. read/write/range lock 을 모두 transaction 종료때까지 보유한다.
### Repeatable Read
transaction 이 시작되는 시점의 snapshot 을 읽는다. range lock 은 관리하지 않으므로, phantom read 가 발생할 수 있다.
### Read Committed
query 가 시작되는 시점의 snapshot 을 읽는다. read lock 을 SELECT query 가 종료되는 시점에 반환하므로, non-repeatable read 도 발생할 수 있다.
### Read Uncommitted 
바로 현 시점의 데이터를 읽는다. 그러므로 dirty read 도 발생할 수 있다.

## 간단하게 Table 로 정리
|Isolation level |Dirty reads|Non-repeatable reads|Phantoms |
|--------------- |-----------|--------------------|---------|
|Read Uncommitted|may occur  |may occur           |may occur|
|Read Committed  |-          |may occur           |may occur|
|Repeatable Read |-          |-                   |may occur|
|Serializable    |-          |-                   |-        |