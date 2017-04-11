Python Weekly 에서 보내주는 뉴스레터에 `decorator` 에 대한 포스트가 있었다. 맞닿뜨린김에 한번 정리하고 넘어가보려고 한다. [Dan Bader 라는 사람의 블로그에 있는 Python Decorators: A Step-By-Step Introduction](https://dbader.org/blog/python-decorators) 이라는 글이다.


## `@` 를 이용한 문법은 syntatic sugar 이다

```python
def target():
    ...

# 데코레이터의 explicit 한 사용
target = decorator(target)
target()
```

```python
@decorator
def target():
    ...

# syntatic sugar 를 사용
target()
```


## Decorator stacking 의 경우 아래에서 위로 실행된다

```python
@top
@bottom
def target():
    ...

# 위 표현은 아래 표현과 동일
target = top(bottom(target))
```


## functools.wraps 를 사용하라

```python
import functools

def decorator(func):
    # func 의 metadata / docstring 등을 wrapper 에서도 접근할 수 있게 된다 
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        ...
    return wrapper
```