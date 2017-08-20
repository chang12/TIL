[Spark 공식 홈페이지의 Downloads 문서](http://spark.apache.org/downloads.html) 에서 메뉴를 선택해서 원하는 소스코드를 다운로드한다.

[Spark 공식 홈페이지의 build 문서](https://spark.apache.org/docs/latest/building-spark.html#buildmvn) 를 따라 소스코드를 빌드한다.

```bash
./build/mvn -DskipTests clean package
```

spark-shell 을 실행해서 정상적으로 Spark 가 빌드되었는지 점검한다.

```bash
spark-shell
```