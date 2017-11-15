[A Scala JDBC connection and SQL SELECT example](https://alvinalexander.com/scala/scala-jdbc-connection-mysql-sql-select-example)

```scala
import java.sql.DriverManager

val url = ...  // RDS 콘솔의 엔드포인트에 앞에 jdbc:mysql:// 달고, 뒤에 슬래시 + 데이터베이스 이름 달고
val username = ...
val password = ...

val connection = DriverManager.getConnection(url, username, password)
val stmt = connection.createStatement

stmt.execute(s"""INSERT INTO ...""")