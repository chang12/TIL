## 개발환경 정의

#### Framework : Django or DRF

지난 토요일 모임때 `Python` 으로 개발하는 것에 대해서는 현영님이랑 공감대가 이뤄졌던 것 같습니다(ㅋㅋ) `Django` 의 강력한 장점 중 하나가 어드민이라고 생각하고, 다만 커스터마이징 하기에는 좀 골치아플 수 있지만, 그렇다고해도 `Flask` 로 개발하고 + 어드민 사이트 바닥부터 다 만드는 것 보다는 `Django` 가 더 좋은 선택이 될 것 같습니다.

이번에는 `Django` 로 (템플릿 엔진 필요없이) 순수하게 API 서버만 만들것이기 때문에 [Django REST Framework (DRF)](http://www.django-rest-framework.org/) 를 사용하면 더 좋을 것 같습니다. 날것의 `Django` 를 쓸때보다 **authentication** 이랑 **(de)serialization** 부분이 더 쓰기 좋았던 기억이 납니다.

프런트엔드 개발자 분들이랑 API 공유하는거 관련해서 [Documenting your API](http://www.django-rest-framework.org/topics/documenting-your-api/) 문서에 나와있는 도구들 중에 하나 골라서 같이 스터디하면서 적용해보고 싶습니다~ (물론 더 좋은 툴이 있다면 그걸로!) 예전에 DRF 로 개발할 때 시도해봤다가 끝내 포기하고 구글닥스로 공유했던 슬픈 기억이 있습니다 ㅋㅋㅋㅋ 이번에는 한번 매듭을 짓고 싶네요. 

#### Deployment

개인적 취향입니다만, 이번에 서비스 개발+배포하는 과정에서 **AWS 클라우드** 서비스들을 오버엔지니어링 수준으로 적극 활용해보면 재밌을 것 같습니다. 지금이야 코드 수정하고 다시 배포할때 그냥 서버 껐다가 다시 키면 되겠지만, 마치 무중단 배포가 절실한 상황인것처럼 배포 프로세스 계획하고, Python 기반이니 [Fabric](http://www.fabfile.org/) 같은 자동화 도구들 함께 스터디하면서 설정해보고 싶습니다.

## 달력 오픈소스

(백엔드에서) 현재 생각으로는 `Python` 의 `datetime` 패키지로도 충분히 기능을 구현할 수 있을거라 생각합니다. 저희 서비스에서 제공하고자 하는 기능이 모두 결정되고나면 더 확실하게 결정할 수 있겠네요.

오픈소스를 찾아보니 `django-scheduler` 라는 프로젝트가 (최근 커밋이 9일전인걸보니) 잘 관리되고있네요. [문서화도 잘 되어있고](http://django-scheduler.readthedocs.io/en/latest/overview.html), [샘플 프로젝트까지 있어서](https://github.com/llazzaro/django-scheduler-sample) 참고해볼만 합니다.