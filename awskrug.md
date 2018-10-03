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

## Zeppelin `spark` interpreter 설정

> java.sql.SQLException: No suitable driver ...

interpreter **Dependencies** 설정에 `mysql:mysql-connector-java:8.0.12` 추가하고 restart. 