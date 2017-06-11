[Python Tips 의 23. Coroutines](http://book.pythontips.com/en/latest/coroutines.html) 문서를 읽고, 신기해서 간단히 실습해봤다.

```python
def coroutine_test():
    print("requested")
    while True:
        msg = yield
        print(msg)

c = coroutine_test()
print("created")
next(c)  # 한번 iterate 해주지 않으면 TypeError 발생
c.send("sent")

# created
# requested
# sent
```

Python Tips 의 다른 포스트를 읽고, Python 의 generator/yield 컨셉에 대해서 간단히 이해했다. 우선 coroutine 의 유용해보이는 점은 함수 호출을 중간에 멈춰놓고 다시 코드를 진행시킬 수 있다는 점이다. Java 에서는 느껴보지 못했던 새로운 느낌의 동시성 제어였다. 좀 더 파보면, 어떤 경우에 유용하게 사용될 수 있을지 알 수 있을 것이다.

곁다리로 더 신기했던 점은 위의 코드 실행에서 **"created"** 가 먼저 출력됬다는 점이다. `coroutine_test` 함수가 호출되면 `yield` 구문을 만날때까지 진행되어 **"requested"** 가 출력되고, 그 후에 **"created"** 가 호출될것으로 추측했는데, 실제로는 **"created"** 가 먼저 출력됬다. **"requested"** 가 출력되는건 `coroutine` 오브젝트를 `next` 함수로 한번 iterate 해준 시점이었다. 애초에 `return` 구문이 없는 `coroutine_test` 함수의 호출을 `c` 에 assign 할 수 있었다는 것 부터가 인터프리터가 `yield` 구문을 특별히 처리해준다는 것을 의미한다. 처음에 `next` 함수를 호출함으로써 `yield` 구문까지 함수 호출을 진행시키는 것으로 추측한다.
