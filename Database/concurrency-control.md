[Concurrency Control Wikipedia 문서](https://en.wikipedia.org/wiki/Concurrency_control#Concurrency_control_mechanisms) 를 보면 다양한 concurrency control mechanism 이 소개되어있다. 이유는 모르겠으나, 예전에 이 문서를 처음 작성할때 아래 lock-based 는 내용이 적혀있었고, multiversion 은 제목만 써있었으므로, multiversion 의 내용만 채운다. Haeinsa 는 optimistic category 에 속한다고 적힌 발표자료를 봤던 기억이 난다.

## lock-based concurrency control

DBMS 에 요청되는 operation 들을 크게 3가지 종류로 나눌 수 있다. **write/read/range** operation 들이 그것이다. lock-based concurrency control 에서는 operation 마다 **lock** 이 정의되고, lock 은 두 가지 종류로 나뉘는 것 같다.
- **"C"** : operation 이 속한 transaction 이 종료될때까지 lock 을 보유한다. 
- **"S"** : operation 이 끝나면 바로 lock 을 반환한다.

그러므로 operation 별로 어떤 종류의 lock 을 사용하는지에 따라서 isolation level 이 분류될 수 있다.

|Isolation level|Write Operation|Read Operation|Range Operation(...where...)|
|-------|---|---|---|
|Read Uncommitted|S|S|S|
|Read Committed|C|S|S|
|Repeatable Read|C|C|S|
|Serializable|C|C|C|

## [multiversion concurrency control](https://en.wikipedia.org/wiki/Multiversion_concurrency_control)

줄여서 MVCC 라고 부른다. MVCC 에서는 lock 을 사용하지 않는다. 대신 DB 에 접근하는 모든 주체는 특정 시점의 **snapshot** 을 읽는다. 데이터를 수정할때는 기존 데이터를 덮어 씌우는게 아니라 obsolete 상태라는걸 표시하고, 다른 곳에 새로운 데이터를 쓴다. 이렇게 하나의 데이터의 여러 버전이 동시에 존재하므로, multi version 이라고 부른다.

transaction 은 **timestamp** 를 지니고, data object 는 **read timestamp 집합**과 **(write timestamp, value) pair 의 집합**을 지닌다.

### transaction 이 data object 를 read

transaction 이 timestamp 를 지니고 있다는걸 기억하자. data object 의 (write timestamp, value) pair 들 중에서 write timestamp 값이 자신의 timestamp 보다 작은것들 중 가장 큰 pair 를 선택해서 그 pair 의 value 를 읽는다. 읽으면서 data object 의 read timestamp 집합에 자신의 timestamp 를 추가한다. **read operation 은 blocking 없이 늘 가능하다는 점이 MVCC 의 장점이다.**

### transaction 이 data object 를 write

**write operation 은 조건을 만족시키는 경우에만 가능하다.** write 하려는 data object 의 read timestamp 집합의 원소들 중에서, transaction 의 timestamp 보다 값이 큰 경우가 없어야 한다. 조건을 만족하는 경우에 transaction 의 timestamp 와 write 하려는 값으로 (write timestamp, value) pair 를 만들어서 data object 의 pair 집합에 추가한다.

이러한 MVCC 는 snapshot isolation 기법과 연관된다. 다양한 concurrency control 중에서 MVCC 만이 유일하게 snapshot isolation 을 완전히 구현할 수 있다고 한다. 또한 **Git** 같은 VCS 에서도 사용된다.
