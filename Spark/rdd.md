```scala
val now = DateTime.now.toDateString.substring(0, 7)
val monthAgo = DateTime.now.plusMonths(-1).toDateString.substring(0, 7)

sc.textFile(s"${nowStr}",s"${monthAgoStr}").count()
```

```scala
val dates = "2017-06-01".toDateTime to DateTime.now by 1.day

dates.map(dt => sc.textFile(s"*${dt.toDateString}*"))
     .reduceLeft(_ ++ _)
     .count()
```

두 코드의 결과는 `171,275` 개로 동일하다. 그러나 전자는 `19s` 가 소요되고 후자는 `3m 58s` 가 소요된다. RDD 의 `++` 연산이 매번 두 RDD 를 `distinct` 하게 합치기 때문에 시간이 오래 걸리는 듯 하다. RDD 를 다룰때 `distinct` 는 정말 필요한 시점에 하면 된다는 교훈을 얻었다. 첫번째 케이스처럼 regex 를 다루지 않더라도, 두번째 케이스에서 `++` 로 `reduceLeft` 하는 대신 `SparkContext` 의 `union` 만 사용하고 마지막에 `distinct` 했어도 첫번째 케이스와 유사한 성능이 나왔을 것이다. 생각없이 코딩하면 안된다.