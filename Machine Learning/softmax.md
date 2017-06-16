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