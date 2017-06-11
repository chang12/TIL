Winning 프로젝트를 Django 에서 Flask 로 이전한다. Django 를 쓸때는 `User` 모델을 제공해줘서 그걸 사용했었다. 따라서 패스워드에 대해 신경 쓸 필요가 없었다. Flask 로 이전하다보니 SQLAlchemy 를 사용하는데, DB 에서 패스워드를 어떻게 저장할지 고민하게 되었다. `sqlalchemy-utils` 패키지에서 패스워드 컬럼을 제공해주는 것 같은데, 우선 vanilla 하게 사용하고 싶어서 제꼈다. `passlib` 패키지는 일반적인 패스워드 관련 도구들을 제공해주므로 유용해보여서 선택했다.

### Django 패스워드

[Password management in Django](https://docs.djangoproject.com/en/1.11/topics/auth/passwords/) 문서를 읽어보니 Django 가 DB 에 저장하는 패스워드는 `$` 로 구분된 문자열이다. DB 에 저장된 패스워드 문자열을 보면 **pbkdf2 sha256** 을 default hasher 로 사용한다.

### passlib

```python
from passlib.hash import pbkdf2_sha256

password_raw = "..."
password_encrypted = pbkdf2_sha256.encrypt(password_raw)  # 암호화해서 DB 에 저장
is_correct = pbkdf2_sha256.verify(password_raw, password_encrypted)  # 올바른 패스워드인지 확인
``` 

패스워드 문자열을 암호화할때 hasher 종류뿐 아니라 `rounds` / `salt` 값도 필요하다. 위의 코드에서 `encrypt` 메서드를 호출할때 값을 지정하지 않았으므로 라이브러리에서 알아서 적당한 값을 사용한다. 암호화된 문자열에 `rounds` / `salt` 값이 적히기 때문에, `verify` 메서드를 호출할때 값을 알 수 있다.
