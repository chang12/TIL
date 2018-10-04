# EMR Cluster

## 설정

* 로깅 : `체크`
* 시작 모드 : `클러스터`
* 릴리스 : `emr`-5.17.0
* Spark: `Spark 2.3.1 on Hadoop 2.8.4 YARN with Ganglia 3.7.2 and Zeppelin 0.7.3`
* 테이블 메타 데이터에서 AWS Glue 데이터 카탈로그 사용 : `체크`
* 인스턴스 유형 : `r3.xlarge`
* 인스턴스 개수 : `3`
* EC2 키 페어 : `awskrug`
* 권한 : `기본값`
* EMR 역할 : `EMR_DefaultRole`
* EC2 인스턴스 프로파일 : `EMR_EC2_DefaultRole`

## SSH 로 Master 노드 접속

* 기본 생성되는 Master 노드의 Security Group 의 22 port public open
* `ssh -i ~/{key_file_path} hadoop@{master_public_dns}`

## SSH Tunneling

* [AWS 문서1 : SSH Tunneling 명령어](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-ssh-tunnel.html)
* [AWS 문서2 : FoxyProxy 로 브라우저에서 Tunneling 통해 접속](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-connect-master-node-proxy.html)
* [EMR 이 제공하는 Web Interface 목록](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-web-interfaces.html) -> Zeppelin 서버가 8890 port

## Zeppelin `spark` interpreter

> java.sql.SQLException: No suitable driver ...

interpreter **Dependencies** 설정에 `mysql:mysql-connector-java:8.0.12` 추가하고 restart.

## Test Data - [fivethirtyeight/uber-tlc-foil-response](https://github.com/fivethirtyeight/uber-tlc-foil-response)

* `uber-trip-data` : S3 에 CSV 파일
* `README.md` 의 Bases : RDS MySQL

## Sample Code

```scala
val s3UberRawDF = spark.read.option("header", true).csv("s3a://awskrug-chang12/uber-raw/uber-raw-data-apr14.csv")
s3UberRawDF.createOrReplaceTempView("s3_uber_raw")

val url = "jdbc:mysql://rds-awskrug.cpjjiovqgjkq.ap-northeast-2.rds.amazonaws.com:3306/awskrug"
val tableName = "uber_base"
val properties = new java.util.Properties()
properties.put("user", "user")
properties.put("password", "password")
val rdsUberBaseDF = spark.read.jdbc(url, tableName, properties)
rdsUberBaseDF.createOrReplaceTempView("rds_uber_base")
```

```sql
select 
    *
from 
    s3_uber_raw join rds_uber_base on s3_uber_raw.Base = rds_uber_base.code
```

# Spark Thrift Server

## 서버 실행

* SSH 로 Master 노드에 접속
* `sudo /usr/lib/spark/sbin/start-thriftserver.sh --master yarn-client`

## beeline 으로 접속

### Port Forwarding

```sh
MASTER_PUBLIC_DNS={master_public_dns}
ssh -i ~/.ssh/awskrug.pem -vvv -N -L 10001:$MASTER_PUBLIC_DNS hadoop@$MASTER_PUBLIC_DNS
```

### Spark Download

EMR Spark 버전에 맞춰 `2.3.1` + Pre-built for Apache Hadoop 2.7 and later 로 다운로드

### beeline 실행

Port Forwarding 할때 선언한 port 와 맞추면 된다.

```sh
~/{SPARK_HOME}/bin/beeline -u "jdbc:hive2://localhost:10001/default"
```

```sh
~/{SPARK_HOME}/bin/beeline
# !connect jdbc:hive2://localhost:10001
# user, password 는 입력없이 blank 로 엔터
```

# 기타

```scala
val options = Map(
    "url" -> "jdbc:mysql://rds-awskrug.cpjjiovqgjkq.ap-northeast-2.rds.amazonaws.com:3306", 
    "dbtable" -> "awskrug.uber_base", 
    "user" -> "awskrug", 
    "password" -> "password"
)
spark.catalog.createTable("test", "org.apache.spark.sql.jdbc", options)

// java.lang.IllegalArgumentException: Can not create a Path from an empty string ...
```
 