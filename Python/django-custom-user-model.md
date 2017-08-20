[Substituting a custom User model](https://docs.djangoproject.com/en/1.11/topics/auth/customizing/#substituting-a-custom-user-model) 문서를 따라서 진행한다.

### app 생성

`python manage.py accounts` 으로 app 생성 (`auth` 는 이미 존재하는 이름이라 사용 불가)


### custom user model 선언

`AbstractUser` 모델을 상속하면서 custom user 모델을 정의한다.

```python
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```
`settings.py` 에 custom User Model 을 명시한다.
```python
# settings.py

...

AUTH_USER_MODEL = 'accounts.User'
```

`email` 필드를 고유한 식별자로 사용하고 싶으므로, [Stack Overflow 답변](https://stackoverflow.com/questions/1160030/how-to-make-email-field-unique-in-model-user-from-contrib-auth-in-django) 를 참고해서 unique constraint 를 추가한다.

```python
class User(AbstractUser):
    class Meta(object):
        unique_together = ('email',)
```

### DB 반영

``` shell
python manage.py makemigrations
python manage.py migrate
```
