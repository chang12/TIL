[참고 : Spark 소스코드에서 빌드하기](../Spark/build-from-source.md)

[Zeppelin GitHub](https://github.com/apache/zeppelin) 에서 소스코드를 clone 한다.

Spark 관련 환경변수를 설정한다. `ex) .bash_profile`

```
export SPARK_HOME=<path-to-spark-directory>
export SPARK_LOCAL_IP=127.0.0.1
```

두번째 환경변수는 Spark 가 원격 클러스터가 아닌 로컬 머신에서 실행된다는 걸 뜻한다.

[Zeppelin 홈페이지의 build 문서](http://zeppelin.apache.org/docs/snapshot/install/build.html) 를 따라 빌드한다. 별도의 option 은 주지 않았다.

```bash
mvn clean package -DskipTests [Options]
```

Zeppelin Server 를 띄운다.

```bash
./bin/zeppelin-daemon.sh start
``` 

브라우저로 8080 포트에 접속해서 Zeppelin 이 정상적으로 실행된 것을 확인한다.

## Spark 버전 관련

Spark 도 소스코드로 빌드했는데, 이 경우 (2017-07 기준) Spark 는 2.3.0 / Zeppelin 은 0.8.0 으로 빌드된다. 둘 다 아직 릴리즈 되지 않은 개발중 상태이다. Zeppelin 도 릴리즈 되지 않은 Spark 2.3.0 에는 호환되지 않기 때문에, spark 인터프리터가 정상적으로 실행되지 않는다. 이를 해결하기 위해 Spark 2.2 버전의 소스코드를 다운로드해서 다시 빌드했었다.
