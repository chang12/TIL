## ACM(AWS Certificate Manager) 에서 SSL/TLS 인증서 발급

API Gateway 에서 custom domain 을 등록하기 위해서는 ACM 에서 발급한 인증서가 필요하다. 무료라길래 거리낌없이 발급받았다. 이때 API Gateway 에서 사용하려면 **Virginia 리전에서** 등록해야한다길래 그렇게 했다. ACM 콘솔에 접속해서 인증받고 싶은 domain 을 입력한다. wildcard 문법으로 `*.fakenerd.kr` 을 적었다. 인증 등록을 신청하면 신청한 도메인의 소유주인지 확인하기 위해서 ACM 에서 도메인 소유주에게 메일을 보낸다. 도메인 소유주가 내 메일로 잘 되있는지 확인하기 위해 OSX 터미널에서 `whois` 명령어를 사용했다.

```bash
> whois fakenerd.kr
```

다행히 예전에 사용하던 네이버 메일로 잘 등록되어있었다. 오랜만에 네이버 메일함을 확인하니 ACM 으로부터 확인 메일이 도착해있었다. 승인을 누르면 등록 절차가 마무리된다.

## API Gateway 에서 custom domain 등록

등록을 원하는 도메인을 입력하고, 사용할 인증서를 선택하면 된다. 매핑도 설정할 수 있는데 테스트를 위해서 base path 를 `test` 로 매핑하고, 자동으로 매핑되도록 stage 는 공란으로 뒀다. 등록하고나면 `cloudfront.net` 의 서브도메인을 발급해준다.

## Route 53 에서 A 레코드 추가

어떤 계정에서 `fakenerd.kr` 에 대한 Hosted Zone 을 만들어뒀고, 이번에 `lambda.fakenerd.kr` 관리는 다른 계정에서 하고 싶었다. 아래 절차로 진행했다.

* `lambda.fakenerd.kr` 에 대한 Hosted Zone 생성 -> NS 레코드가 기본 생성되고 네임 서버 도메인 4개가 얻어진다.
* 얻은 도메인 4개를 가지고 `fakenerd.kr` 에 대한 Hosted Zone 에서 `lambda.fakenerd.kr` 에 대한 NS 레코드를 생성한다.
* 위의 API Gateway 작업에서 얻은 `cloudfront.net` 도메인을 기록해두고,
* 다시 `lambda.fakenerd.kr` 에 대한 Hosted Zone 으로 돌아와서, `lambda.fakenerd.kr` 에 대한 A 레코드를 만들면서 **Alias** 에 `cloudfront.net` 도메인을 넣고, 생성한다.

## 결론

수 분이 지나고나서 `https://lambda.fakenerd.kr/test/prod/test` 를 때려보면, 원래 `https://lddror6mx0.execute-api.ap-northeast-2.amazonaws.com/prod/test` 에 물려있던 Lambda 응답이 내려오는 것을 확인할 수 있다.