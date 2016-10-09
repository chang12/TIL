## Module
모듈(module)은 상수, 함수, 클래스들의 모음이다. 아래처럼 `utils.py` 모듈에 몇가지 상수, 함수, 클래스들을 정의해놓으면 다른 모듈에서 이를 `import` 하고 사용할 수 있다.
```python
# utils.py
MY_NAME = "Changhyun Lee"

def print_name():
	print("My name is {} (by function)".format("changhyun"))


class Name:
	def __init__(self, name):
		self.name = name

	def print_name(self):
		print("My name is {} (by class)".format(self.name))
```
또한 모듈은 독자적인 **스크립트(script)**의 기능을 수행할 수 있다. 아래 `use.py`에 다른 모듈의 상수, 함수, 클래스를 import 해서 사용하는 예시와 독자적인 스크립트의 기능을 수행하는 예시를 함께 담았다. 

`if` 문의 조건은 `python use.py` 명령어로 스크립트로써 호출되는 경우에 유효하다. 스크립트를 `if` 문 바깥에 놓으면, 스크립트로써 호출될때 뿐 아니라 다른 모듈에서 `import` 할때도 실행된다. `utils.py`에서 `MY_NAME` 상수를 선언한것도 일종의 스크립트이다.
```python
# use.py
import utils

def print_name_two_ways():
	utils.print_name()
	name = utils.Name(utils.MY_NAME)
	name.print_name()

if __name__ = "__main__":
	print_name_two_ways()
```
```
> python use.py
My name is Changhyun Lee (by function)
My name is Changhyun Lee (by class)
```
## Package
패키지는 모듈들의 집합이다. 모듈들이 모여서 패키지를 이루고, 다시 패키지와 모듈들이 모여서 더 큰 패키지를 구성하는 식이다. 계층 구조가 복잡해져도, 도트(.)를 이용해서 모듈들을 편리하게 관리할 수 있다. 
### PYTHONPATH
패키지에 대한 얘기를 하려면 우선 `PYTHONPATH` 환경변수를 이해해야한다. `PYTHONPATH` 환경변수는 이름에 나타나듯 여러 Path 들의 집합체이다. 각각의 Path 는, 다른 패키지나 모듈을 `import` 했을 때 Python 인터프리터가 탐색하는 루트 디렉토리가 된다. 그러므로 새로운 패키지를 만든다면, 그 패키지가 속한 디렉토리는 `PYTHONPATH`에 속하는 경로여야 한다.
### \_\_init\_\_.py
패키지는 `__init__.py` 라는 이름의 파일을 포함해야 한다. 인터프리터가 `__init__.py` 의 존재 여부로 디렉토리가 패키지인지 분간하기 때문이다. `__init__.py` 는 그 자체로 또한 모듈이므로, 상수/함수/클래스의 정의가 가능하다. 패키지를 정의하는데에는 빈 `__init__.py` 로도 충분하다. 하지만 `*`를 활용한 특별한 `import`를 위해서는 빈 파일로는 부족하다.

어떤 패키지에서 `import` 할 항목들이 많을 경우 이를 일일히 하나의 `import` 선언으로 만드는 것은 번거로울 수 있다. 그 경우 `*`을 활용한 전체 `import`가 유용할 수 있는데 이를 위해서는 패키지의 `__init__.py`에 `__all__` 이라는 상수 리스트가 선언되어있어야 한다. `*`을 활용한 `import`를 할 경우 `__init__.py`에 정의된 상수, 함수, 클래스, 그리고 `__all__` 상수 리스트의 원소를 이름으로 하는 하위 패키지와 모듈을 `import` 하기 때문이다. 예시를 위해 아래와 같은 디렉토리 구조를 가정하자.

```
project/
	package1/
		__init__.py
		module1.py
		module2.py
		module3.py
	main.py
```
```python
# package1/__init__.py
__all__ = ['module1', 'module3', ] # module2는 빠져있다.
```
이 경우 `main.py` 모듈에서 `from package1 import *`로 `package` 패키지의 요소들을 전체 `import` 해도 `module2`의 요소들은 사용할 수 없다. 여담으로 모듈의 경우에는 별다른 선언 없이 `from 모듈_이름 import *` 해도 해당 모듈의 모든 요소들을 `import` 할 수 있다.
## from / import
`from`과 `import`를 활용해서 다른 패키지/모듈의 요소를 가져다 쓰는 여러 방식들에 대한 케이스 스터디를 해보자. 아래와 같은 디렉토리 구조를 가정한다. **project** 디렉토리는 `PYTHONPATH` 환경변수에 포함되어있다.
```
project/
	package/
		__init__.py
		subpackage/
			submodule.py
		module.py
```
```python
# submodule.py
def sub_func():
	...
```
`module.py`에서 `sub_func` 함수를 포함하고 싶다면 3가지 다른 방법이 가능하다.
```python
import package.subpackage.submodule
package.subpackage.submodule.sub_func()
...
from package.subpackage import submodule
submodule.sub_func()
...
from package.subpackage.submodule import sub_func
sub_func()
```
아래 3가지 방법은 불가능하다. 처음 두 가지 방법이 불가능한 이유는, 최종 `import` 대상이 패키지인 경우 해당 패키지의 `__init__.py`에 정의된 상수, 함수, 클래스에만 접근할 수 있기 때문이다. 최종 `import` 대상에서 도트를 이용해서 한단계 더 하위의 패키지/모듈에 접근하는 것은 불가능하다. 마지막 방법이 불가능한 이유는 `import` 문에서 도트를 이용해 접근할 경우 가장 마지막 항목은 패키지나 모듈이어야 하기 때문이다.
```python
# import 후에 도트로 하위 패키지나 모듈에 접근하는 것은 불가능!
import package
package.subpackage.submodule.sub_func()
...
from package import subpackage
subpackage.submodule.sub_func()
...
# import 에 도트를 쓸 경우 가장 마지막은 패키지 혹은 모듈만!
import package.subpackage.submodule.sub_func
sub_func()
```
마지막으로 **상대경로**에 대해서 얘기할 것이다. 지금까지의 얘기는 모두 `PYTHONPATH` 환경변수에 기반한 **절대경로**에 대한 얘기였다. 위 디렉토리 구조에서 `submodule.py`에서 `module.py`에 정의된 함수를 사용하고 싶다고 가정하자.
```python
# module.py
def func():
	...
```
상대경로를 위한 2가지 도구가 있다. `..`은 부모 디렉토리로의 접근을, `.`은 현재 디렉토리로의 접근을 허용한다. 그러므로 `submodule.py`에서 `module.py`의 `func` 함수를 `import` 하고싶다면 상대경로를 사용해서 아래와 같이 가능하다.
```python
from ..module import func
func()
```