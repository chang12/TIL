간단한 DataFrame 을 만들어서 csv 파일로 저장하고, 다른 schema 를 명시해서 읽어본다.

```scala
spark.sparkContext.parallelize(Seq(
	("a", 1),
	("b", 2),
	("c", 3)
))
	.toDF("c1", "c2")	
	.write
	.option("header", true)
	.csv(S3_PATH)
```

```scala
spark
	.read
	.schema(
	    StructType(
	        StructField("c1", StringType, nullable=true) ::
	        StructField("c3", StringType, nullable=true) ::
	        Nil))
	.option("header", true)
	.csv(S3_PATH)
	.show
```

```
+---+---+
| c1| c3|
+---+---+
|  a|  1|
|  b|  2|
|  c|  3|
+---+---+
```

`c1` 은 잘 읽고, `c2` 는 무시하고, `c3` 는 null 로 채워지기를 기대했다. 기대와 달리 `c2` 를 `c3` 로 읽어버린다.

csv 대신 parquet 포맷으로 저장하면 결과가 달라질까?

```scala
spark
	.read
	.schema(
	    StructType(
	        StructField("c1", StringType, nullable=true) ::
	        StructField("c3", StringType, nullable=true) ::
	        Nil))
	.parquet(S3_PATH)
	.show
```

```
+---+----+
| c1|  c3|
+---+----+
|  a|null|
|  b|null|
|  c|null|
+---+----+
```

원하는 결과를 얻을 수 있다.