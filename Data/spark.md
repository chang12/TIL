# DataFrameReader 로 S3 파일 read

## hadoop-aws

`s3a://...` 로 S3 endpoint 를 path 로 넘겼을때 에러가 발생한다.

> java.lang.RuntimeException: java.lang.ClassNotFoundException: Class org.apache.hadoop.fs.s3a.S3AFileSystem not found ...

`SPARK_HOME` 아래 jars 디렉토리에 `hadoop-aws` 라이브러리를 추가하고, zeppelin interpreter 를 재시작하면 된다.

```sh
wget http://central.maven.org/maven2/org/apache/hadoop/hadoop-aws/2.7.3/hadoop-aws-2.7.3.jar
```

## aws-java-sdk

다시 read 하면 다른 에러가 발생한다.

> java.lang.NoClassDefFoundError: com/amazonaws/AmazonServiceException ...

마찬가지로 jars 디렉토리에 `aws-java-sdk` 라이브러리를 추가하고, zeppelin interpreter 를 재시작한다.

```sh
wget http://central.maven.org/maven2/com/amazonaws/aws-java-sdk/1.7.4/aws-java-sdk-1.7.4.jar
```

## 400 Bad Request

서울 리전의 Signature Version 관련 에러가 발생한다. 아래 코드를 실행해서 해결할 수 있다.

```scala
import com.amazonaws.SDKGlobalConfiguration

System.setProperty(SDKGlobalConfiguration.ENABLE_S3_SIGV4_SYSTEM_PROPERTY, "true")
sc.hadoopConfiguration.set("fs.s3a.endpoint", "s3.ap-northeast-2.amazonaws.com")
```

