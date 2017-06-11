Flask 에서 request local 한 `request` 오브젝트를 제공한다. HTTP POST 요청을 날릴때 파라미터를 실어 보내면 서버에서 이 값을 획득해서 처리해주게 된다. 이때 `request` 오브젝트의 `form` 프로퍼티에서 값을 뽑아오기 위해서는 HTTP 요청을 날리는 클라이언트에서 **Content-Type** 을 **x-www-form-urlencoded** 로 설정해줘야 한다.  

```python
from flask import request


def api():
    name = request.form["name"]
    age = request.form["age"]
    ...

```