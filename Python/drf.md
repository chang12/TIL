## Serializer

### `read_only` / `write_only`

* `Serializer` 를 처음 접했을때는 Django 때 `Form` / `ModelForm` 을 쓰던게 생각나서 `ModelSerializer` 를 많이 썼다. vanila 한 CRUD 연산들은 이걸로도 충분했다.
* `ModelSerializer` 의 CRUD 연산의 동작을 커스터마이징 하고 싶어졌고, CRUD 메서드들을 오버라이딩하기 시작했다. 
* 오버라이딩이 꼭 필요한 경우들이 있다. 간단한 예로, authentication 레이어에서 context 에 넣어준 `User` 에서 값을 뽑아쓰는 경우등이 있다. 하지만 꼭 필요하지 않은 경우도 있는데, 클라이언트에게 내려주는 응답에 `Model` 의 일부 필드만 쓰고 싶거나, 필드가 아닌 다른 값을 넣고 싶은 경우다. 처음에는 이 경우에도 `create` 와 `to_representation` 메서드를 오버라이딩해서 해결했었다.
* Nexters 에서 프로젝트를 더 진행하면서 DRF 를 계속 쓰다보니 이런 코드 구조가 마음에 안들었다. 커스터마이징이 두 메서드에 나눠 구현되다보니 어느쪽에 두는게 더 맞는지에 대한 모호함이 생겼기 때문이다.
* 그러던 중 `Serializer` 필드의 `read_only` / `write_only` 옵션에 대해 알게됬고, 이를 적극 활용하는게 더 DRF 스러운 코딩이라고 판단했다.

```python
# 기본적으로는 Serializer 의 필드를 선언할때 파라미터로 선언한다.
password_check = serializers.CharField(max_length=128, write_only=True)

# ModelSerializer 를 사용해서 필드를 명시적으로 선언하지 않는 경우 Meta 클래스에 옵션을 선언한다.
class Meta:
    ...
    extra_kwargs = {'password': {'write_only': True}}
``` 