MySQL Server 와 storage engine 사이의 인터페이스를 `handler` 라고 한다.

* [MySQL Internals Manual  /  Writing a Custom Storage Engine  /  Overview](https://dev.mysql.com/doc/internals/en/custom-engine-overview.html)
* [MySQL Internals Manual  /  Writing a Custom Storage Engine  /  Creating the handlerton](https://s3.console.aws.amazon.com/s3/buckets/analytics-between-dashboard/kochava/reports-bulk-monthly/create_relationship%252Cfirst_open_between%252Crelationship_created%252Csign_up/?region=ap-northeast-1&tab=overview)

MySQL Client 가 SQL 문자열을 넘기면, parser 와 optimizer 를 거쳐 plan 으로 변환된다.