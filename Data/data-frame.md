## Window

Databricks 블로그의 [Introducing Window Functions in Spark SQL](https://databricks.com/blog/2015/07/15/introducing-window-functions-in-spark-sql.html) 포스트를 읽어보면 Window 관련 기능들의 용례와 사용법에 대해 핵심적인 내용들을 파악할 수 있다.

### `percent_rank`

매니저님이 `DataFrame` 사용한 코드 예시를 읽으면서 SQL 의 `Window` 에 대해 처음 알게되었다. 그룹으로 묶고 / 정렬하고 / 순위를 매길때 무척 유용해보인다. SQL 에 존재하므로, `DataFrame` 에도 존재한다. `RDD` 였다면 계산하기 힘들었을 것이다.

```scala
// 상위 10% 뽑기
import org.apache.spark.sql.expressions.Window

val window = Window.partitionBy($"grouping_col").orderBy($"ordering_col".desc)

df.select($"*", percent_rank.over(window).alias("percent")).where($"percent" < 0.1)
```

### `row_number`, `rank`, `dense_rank`

다른 순서 관련 function 들도 유용하다. 3가지가 비슷하면서도 각각의 특색을 지니고 있다. 아래 예제로 한눈에 확인해볼 수 있다.

```scala
val w = Window.partitionBy("id").orderBy("value")

spark
    .sparkContext
    .parallelize(
        Seq(
            ("a", 1),
            ("a", 1),
            ("a", 3),
            ("a", 4)
        )
    )
    .toDF("id", "value")
    .withColumn("row_number", row_number.over(w))
    .withColumn("rank", rank.over(w))
    .withColumn("dense_rank", dense_rank.over(w))
    .show
```

|  id|value|row_number|rank|dense_rank|
|---:|----:|---------:|---:|---------:|
|   a|    1|         1|   1|         1|
|   a|    1|         2|   1|         1|
|   a|    3|         3|   3|         2|
|   a|    4|         4|   4|         3|

### `lag`, `lead`

window 와 함께 사용할 수 있는 `lag`, `lead` 함수도 유용하다. `partitionBy` 로 선언한 partition 안에서, `orderBy` 로 선언한 순서대로 정렬된 상태에서, 몇칸 앞/뒤의 데이터를 접근할 수 있다. 로그를 분석할때 같은 action 이 여러번 일어났을때나, 같은 유저의 여러 다른 action 간의 관계를 파악하고자할때 유용하다.

### `rowsBetween`, `rangeBetween`

time series 데이터를 분석할때 특히 유용하다. 시간순으로 정렬했을때 최근 몇번 / 이후 몇번 같은 접근이 필요할때는 `rowsBetween`, 최근 몇분 / 이후 몇분 같은 접근이 필요할때는 `rangeBetween` 이 유용하다.

## JDBC

Spark 문서중에서 [JDBC To Other Databases](https://spark.apache.org/docs/latest/sql-programming-guide.html#jdbc-to-other-databases) 파트를 보면 `DataFrame` 에서 JDBC 를 통해 MySQL 등의 데이터베이스에 읽고/쓰는 방법을 알 수 있다.

```scala
spark
	.read
	.format("jdbc")
	.option("url", "jdbc:mysql://my_database_endpoint:3306/my_database_name")
	.option("dbtable", "my_table_name")
	.option("user", "my_user_name")
	.option("password", "my_password")
	.load
```

```
sc.parallelize(...)
  .toDF("field1", "field2", ...)
  .write
  .mode("append")
  .format("jdbc")
  .option("url", "jdbc:mysql://my_database_endpoint:3306/my_database_name")
  .option("dbtable", "my_table_name")
  .option("user", "my_user_name")
  .option("password", "my_password")
  .save
```
