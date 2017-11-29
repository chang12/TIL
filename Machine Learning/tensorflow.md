## TensorBoard

#### Jupyter Notebook 환경에서 `InvalidArgumentError`

`train` 함수를 `utils.py` 에 작성해두고 notebook 에서는 module 을 import 해서 사용했다. notebook kernel 을 띄우고, 최초로 `train` 함수를 실행했을때는 정상 동작하는데, 재실행하면 placeholder 에 값을 넣지 않았다면서 `InvalidArgumentError` 를 발생시켰다. `feed_dict` 인자를 제대로 넘겼기 때문에 이해할 수 없는 예외였다. [Stack Overflow 답변을](https://stackoverflow.com/questions/39356714/using-tensorboard-with-jupyter-notebooks) 참고해서 `train` 함수가 호출될때마다 graph 를 reset 하니 해결되었다.

```python
import tensorflow as tf
...

def train(num_epoch, ...):
	tf.reset_default_graph()
	
	...
```

#### Scalar 값이 제대로 안찍히는 문제

epoch 을 100회 반복하면서 매 epoch 마다 `tf.summary.FileWriter` 의 `add_summary` 메서드를 호출했다. 그러나 TensorBoard 에 접속해서 Scalar 를 조회해보면 100번의 epoch 에 대한 loss 값이 안찍히고 앞에 12회 정도만 찍히는 현상이 계속 반복되었다. [FileWriter](https://www.tensorflow.org/api_docs/python/tf/summary/FileWriter) 쪽 문서를 읽다가 명시적으로 호출할 수 있는 `flush` 메서드가 있길래 반갑게 사용하니 모든 epoch 의 값을 잘 찍을 수 있었다.
