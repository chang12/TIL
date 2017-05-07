Java 8 API 를 사용하면 datetime 을 다루는게 편리했어다. `System.currentTimeMillis()` 를 호출하면 바로 현재 시각을 unix timestamp 로 얻을 수 있었고, `Instant`, `ZoneId`, `ZonedDateTime` 등의 객체를 활용하면 원하는 변환 작업을 수월하게 진행할 수 있었다. Python 에서도 이러한 작업은 필요했고, 간단히 정리해봤다.

### 현재 시각을 unix timestamp 로 얻기

`time` 모듈을 사용할 수 있었다. Java 의 `System.currentTimeMillis` 는 이름에서 드러나듯이 msec 단위를 사용했는데 반해  `time` 모듈은 sec 를 기본 단위로 사용하고, sec 이하 값을 소수점으로 제공했다.

```python
import time

time.time() #  1494159415.411155
int(time.time() * 1000) #  1494159677148
```

### unix timestamp 를 Time Zone 별 datetime 으로 변환

Python 에서 timezone 을 다루기 위해서는 `pytz` 패키지가 필요하다. 다운로드한다. `pytz` 패키지의 `timezone` 모듈을 사용해서 원하는 이름의 `tzinfo` 를 상속하는 object 를 만들 수 있다. `tzinfo` object 를 만들고나서는 `datetime` 모듈의 `fromtimestamp` 메서드를 사용하면 된다. `pytz` 패키지에 대해서는 [어떤 한국분의 블로그 포스트를](http://technote.kr/202) 참고했다.

```python
from datetime import datetime

from pytz import timezone

epoch_sec = 1494160551

kst_tz = timezone("Asia/Seoul")
utc_tz = timezone("UTC")

kst_datetime = datetime.fromtimestamp(epoch_sec, tz=kst_tz)
utc_datetime = datetime.fromtimestamp(epoch_sec, tz=utc_tz)

kst_datetime #  datetime.datetime(2017, 5, 7, 21, 35, 51, tzinfo=<DstTzInfo 'Asia/Seoul' KST+9:00:00 STD>)
utc_datetime #  datetime.datetime(2017, 5, 7, 12, 35, 51, tzinfo=<UTC>)
```
