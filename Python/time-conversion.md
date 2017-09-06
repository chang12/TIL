Java 8 API 를 사용하면 datetime 을 다루는게 편리했었다. `System.currentTimeMillis()` 를 사용해서 unix timestamp 를 ms 로 얻을 수 있었고, `Instant`, `ZoneId`, `ZonedDateTime` 등의 객체를 사용해서 원하는 변환 작업을 진행할 수 있었다. Python 에서도 같은 작업이 필요했고, 간단히 정리해봤다.

## 현재 epoch sec 획득

`time` 모듈을 사용한다. Java 의 `System.currentTimeMillis` 는 이름처럼 msec 단위를 반환하지만 `time` 모듈은 sec 단위로 반환하면서, sec 이하 값을 소수점으로 제공한다.

```python
import time

time.time()  # 1494159415.411155
int(time.time() * 1000)  # 1494159677148
```

## epoch sec 를 datetime object 으로 변환

timezone 을 다루기 위해 `pytz` 패키지를 사용한다. `pytz` 패키지의 `timezone` 모듈을 사용해서 원하는 이름의 `tzinfo` 를 상속하는 object 를 만든다. 그러고나서 `datetime` 모듈의 `fromtimestamp` 메서드를 사용한다. `pytz` 패키지에 대해서는 [어떤 한국분의 블로그 포스트를](http://technote.kr/202) 참고했다.

```python
from datetime import datetime

from pytz import timezone

epoch_sec = 1494160551

kst_tz = timezone("Asia/Seoul")
utc_tz = timezone("UTC")

kst_datetime = datetime.fromtimestamp(epoch_sec, tz=kst_tz)
utc_datetime = datetime.fromtimestamp(epoch_sec, tz=utc_tz)

kst_datetime  # datetime.datetime(2017, 5, 7, 21, 35, 51, tzinfo=<DstTzInfo 'Asia/Seoul' KST+9:00:00 STD>)
utc_datetime  # datetime.datetime(2017, 5, 7, 12, 35, 51, tzinfo=<UTC>)
```

## datetime 문자열 -> datetime object -> epoch sec 으로 변환

Django 에서 데이터를 JSON dump 하면, `DateTimeField` 타입의 필드는 다음과 같은 포맷으로 직렬화 된다.

```
2016-05-19T15:59:27.471Z
```

Python 에서 datetime 문자열을 datetime object 로 역직렬화할때는 `datetime` 모듈의 `strptime` 메서드를 사용한다. 위의 포맷을 나타내는 포맷팅 문자열은 `%Y-%m-%dT%H:%M:%S.%fZ` 이다.

`strptime` 메서드가 반환하는 `datetime` object 는 `tzinfo` 필드가 채워지지 않은 naive `datetime` object 이다. 값의 맥락을 살펴봤을때 Django 의 직렬화 문자열은 UTC timezone 의 값이므로 명시적으로 UTC timezone 을 선언해준다.

`datetime` object 를 epoch sec 로 바꿀때는 `timestamp` 메서드를 호출하면 된다.

```python
from dateime import dateime

import pytz

datetime_str = "2016-05-19T15:59:27.471Z"
naive_dt = datetime.strptime(datetime_str, "%Y-%m-%dT%H:%M:%S.%fZ")
utc_dt = pytz.utc.localize(naive_dt)

utc_dt.timestamp()  # 1463673567.471
```

그러므로 `int()` 로 소수점 이하를 버려주면 DB 에 넣을 수 있는 값이 된다.

## time 파싱 -> 시간 간격

```python
from dateutil.parser import parse  # python-dateutil 모듈을 사용해서 간편하게 파싱할 수 있었다.

def diff(s1, s2):
    # 18:26:30.375 
    t1 = parse(s1)
    t2 = parse(s2)
    return (t2-t1).seconds * 1000 + (t2-t1).microseconds / 1000  # 이 부분은 정말 아쉽다...
```