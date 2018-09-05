# Zeppelin Download

작성일인 2018.08.27 기준 최신 릴리즈 버전은 0.8.0 이다. 하지만 불안해보여서 **0.7.3** 을 선택했다. [Download Apache Zeppelin](https://zeppelin.apache.org/download.html) 페이지에서 zeppelin-0.7.3-bin.all.tgz 를 받았다.

```sh
wget http://archive.apache.org/dist/zeppelin/zeppelin-0.7.3/zeppelin-0.7.3-bin-all.tgz
tar -xvzf zeppelin-0.7.3-bin-all.tgz
cd zeppelin-0.7.3-bin-all
```

# SPARK_HOME 설정

zeppelin 과 비슷하게, pre-built spark 를 download 하고 (e.g. spark-2.3.1-bin-hadoop2.7.tgz) 압축을 풀어주자. zeppelin 의 [1. Export SPARK_HOME](https://zeppelin.apache.org/docs/latest/interpreter/spark.html#1-export-spark_home) 문서를 참고해서 `zeppelin-env.sh` 파일을 만들고, 위에서 download 받은 경로를 `SPARK_HOME` 로 선언한다.

# Zeppelin 서버 실행

```bash
# logs 디렉토리에 로깅하고, run 디렉토리에 pid 적네.
./bin/zeppelin-daemon.sh start
```

서버를 실행할때 변수를 선언할 수도 있다.

```bash
SPARK_HOME=/.../spark ./bin/zeppelin-daemon.sh start
```

# 브라우저로 확인

* 8080 포트로 잘 접속되는지 확인한다. 
* spark interpreter 로 note 를 만들고, `System.getenv("SPARK_HOME")` 로 `SPARK_HOME` 이 제대로 설정됬는지 확인한다. 