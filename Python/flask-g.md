**access token** 기반의 authentication 을 API 에 적용하고 싶었다. jwt 라이브러리로 만든 access token 을 decode 하고, payload 를 까서 유저 ID 값을 획득하고, 적당한 위치에 넣어주는 로직을 데코레이터로 빼고, 필요한 API 함수마다 달아서 사용하고 싶었다. Flask 가 제공하는 `g` 오브젝트를 사용했다.

```python
from functools import wraps

from flask import g


def login_required(f):
    @wraps
    def decorated_func(*args, **kwargs):
        ...
        g.user_id = user_id  # 값을 넣어주고,        
    return decorated_func


@login_required
def api_need_authentication():
    user_id = g.user_id  # 값을 뽑아서 사용한다.
    ...

```