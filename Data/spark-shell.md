spark-shell 을 이용해서 spark application 을 시작하는 가장 간단한 명령어는 아래와 같다.

```sh
/path-to-spark/bin/spark-shell \
	--master spark://path-to-spark-master:7077 \
	--jars /path-to-some-jar-file.jar
```

spark 코드는 다른 편집기에 미리 작성해두고, shell 에서 `:paste` mode 로 진입해서 복붙하면 편리하다.