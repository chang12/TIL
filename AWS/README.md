* **Kinesis** : stream 들을 보관하는 역할로 사용한다. [공식 문서에서 요금 사용 예시를 시나리오로 설명한 내용](https://aws.amazon.com/ko/kinesis/streams/pricing/)을 읽으면 느낌을 잘 받을 수 있다.

## Linux 가상 머신 시작
```
AMI 는 소프트웨어 구성(예: 운영 체제, 애플리케이션 서버 및 애플리케이션)이 포함된 템플릿입니다.
```
* AMI 는 **Ubuntu Server 16.04 LTS** 를 선택했다. 14.04 LTS 를 할까 고민했었다.
* Step 3 에서 Monitoring 을 설정해보고 싶었는데, CloudWatch 는 추가 비용이 발생한다.
* EBS volume 과 instance store volume 의 차이점이 궁금하다. 인스턴스를 한번 생성한 뒤에 EBS volume 은 추가할 수 있지만, instance store volumne 은 불가능하다.

## What is Amazon EC2?
* instance store volume 은 instance 가 terminated 되면 사라진다. 이에 반해 EBS(Elastic Block Store) 는 persistent 하다.
* VPCs(Virtual Private Clouds) 는 virtual network 이다. AWS cloud 의 나머지 부분과 논리적으로 격리된 virtual network 를 만들 수 있다.

## Getting Started with Amazon EC2 Linux Instances
TODO 