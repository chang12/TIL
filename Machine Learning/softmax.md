```python
# 뉴럴 네트워크를 통과해서 나온 스코어. 보통 M x D 의 2D array. (M = batch 크기)
score = ... 
M = score.shape[0] 

# exp() 값의 over-flow 를 방지하기 위해 shift. softmax 값이 바뀌지 않는 성질을 이용
# 2D array 에 max 를 취하면 1D array 가 되어 원하는대로 broadcast 되지 않음 -> reshape
score_norm = score - np.reshape(np.max(score, axis=1), [M, 1])
prob = np.exp(score_norm) / np.reshape(np.sum(np.exp(score_norm), axis=1), [M, 1])

return prob
```

* 임의의 실수값인 **score** 를 [0, 1] 범위의 **probability** 로 변환하는데에 많이 사용된다.
* 그러므로 softmax 레이어는 뉴럴 네트워크에서 마지막 최종 결과값 레이어 뒤에 붙는다. 
* 뒤이어 보통 cross entropy 레이어를 사용해서 **cost** 를 계산한다.

## `softmax` 는 왜 이름이 "softmax" 일까?

회사 팀원분들과 스터디를 하는데 문득 "softmax" 라는 이름이 붙은 이유가 궁금해졌다. 구글링해보니 [Quora 답변이](https://www.quora.com/Why-is-softmax-activate-function-called-softmax) 가장 위에 떴는데, 첫번째 답변을 읽어보니 납득이 됬다.

* softmax "function" 이 먼저 있었다. `max` function 과 유사한데, 경계에서 미분 불가능한 `max` 와 달리, 부드럽게 꺾이기 때문에 "soft"max function 이다.
* softmax activation 의 수식 꼴이 softmax function 과 거의 동일하기 때문에 softmax 라고 이름이 붙었다.
