## Window

매니저님이 `DataFrame` 사용한 코드 예시를 읽으면서 SQL 의 `Window` 에 대해 처음 알게되었다. 그룹으로 묶고 / 정렬하고 / 순위를 매길때 무척 유용해보인다. SQL 에 존재하므로, `DataFrame` 에도 존재한다. `RDD` 였다면 계산하기 힘들었을 것이다.

```scala
// 상위 10% 뽑기
import org.apache.spark.sql.expressions.Window

val window = Window.partitionBy($"grouping_col").orderBy($"ordering_col".desc)

df.select($"*", percent_rank.over(window).alias("percent")).where($"percent" < 0.1)
```