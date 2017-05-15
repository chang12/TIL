그동안 Python 코드로 HTTP 요청해서 데이터를 받아오는 작업이 필요했을때면 **requests** 패키지를 일단 설치하고 시작했었다. 하지만 Python built-in 으로도 HTTP 요청을 때려서 데이터를 받아오는 작업은 그리 번거롭지 않았다. 다른 분의 코드를 읽다가 `urllib.request` 모듈의 `urlopen` 함수를 사용하는 것을 봤다. 이와 연관된 기반 내용을 이해하고싶어서 검색하다보니 [Python HOWTO 문서중에 하나가](https://docs.python.org/3/howto/urllib2.html) 정확히 연관되어있었다. 읽어보니 `urllib.request` 모듈의 `urlopen` 함수말고 `urlretrieve` 함수도 비슷하지만 살짝 다른 용도로 사용가능했다. 

`urlopen` 함수는 `http.client.HTTPResponse` 를 반환한다. 상속하는 `_IOBase` 클래스에서 `__enter__` 와  `__exit__` 메서드를 구현하므로 Python 의 with 구문으로 처리할 수 있다. `read` 메서드로 읽으면 `bytes` 인스턴스를 반환한다.

```python
with urlopen("http://python.org") as response:
    html = response.read()  # bytes 반환
```

`urlretrieve` 함수는 `str` / `HTTPMessage` 의 튜플을 반환한다. `str` 값은 HTTP 응답을 적은 파일의 경로를 나타낸다. 그러므로 값이 저장장치에 저장되기 때문에 여러번 처리할 수 있다.

```python
local_filename, headers = urlretrieve("http://python.org")
```

경우에 따라 둘 중 한가지 방법을 사용할 수 있을 것이다.
