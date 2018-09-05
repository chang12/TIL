# 문제 1 : aws-java-sdk

## 현상

`AmazonS3Client#listObjects` 를 호출했는데 예외가 발생했다.

> The authorization mechanism you have provided is not supported. Please use AWS4-HMAC-SHA256.

## 원인

[Authenticating Requests (AWS Signature Version 4)](https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-authenticating-requests.html) 를 읽어보니 2014년 1월 30일 이후 신규 region s3 는 Signature Version 4 만 지원하고, [Now Open – AWS Asia Pacific (Seoul) Region](https://aws.amazon.com/blogs/aws/now-open-aws-asia-pacific-seoul-region/) 뉴스는 2016년 1월에 있었다.

## 해결

system property 값을 추가하고, endpoint 를 명시하니 해결됬다.

```scala
import com.amazonaws.SDKGlobalConfiguration

System.setProperty(SDKGlobalConfiguration.ENABLE_S3_SIGV4_SYSTEM_PROPERTY, "true")
val client = new AmazonS3Client()
client.setEndpoint("s3.ap-northeast-2.amazonaws.com")
```

# 문제 2 : Spark DataFrameReader

`DataFrameReader` 로 `s3a://` 경로의 파일을 읽을때는 직접 `AmazonS3Client` 를 만들지 않기 때문에 endpoint 를 어떻게 설정해야할지 고민됬다.

## 해결

[Custom S3 endpoints with Spark](https://gist.github.com/tobilg/e03dbc474ba976b9f235) 라는걸 참고해서 해결했다.

```scala
import com.amazonaws.SDKGlobalConfiguration
System.setProperty(SDKGlobalConfiguration.ENABLE_S3_SIGV4_SYSTEM_PROPERTY, "true")
sc.hadoopConfiguration.set("fs.s3a.endpoint", "s3.ap-northeast-2.amazonaws.com")
```