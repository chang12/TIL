## DataFrame 의 byte 크기 얻는 방법
[SizeEstimator](https://spark.apache.org/docs/latest/api/scala/#org.apache.spark.util.SizeEstimator$) 를 이용해서 `DataFrame` 의 byte 단위 크기를 얻을 수 있다. spark 클러스터 worker 머신의 메모리 크기와 비교해서, cache 하기에 적절한지 결정할 수 있다. cache 하고/안하고에 따라서 다양한 action 들의 소요시간이 크게 차이나기 때문이다.

```scala
import org.apache.spark.util.SizeEstimator

val df = spark.read.json("s3a://...")
SizeEstimator.estimate(df) / (1024 * 1024)  // MB 단위 크기
```