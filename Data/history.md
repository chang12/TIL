* [Standford cs231n 스터디](https://github.com/Industrial-Functional-Agent/cs231n) with @bellatoris

### 2017-09-15

* 지난 주말에 찾았던 **char-rnn-tensorflow** 프로젝트를 fork 하고 회사 머신에 clone 해서 만져보기 시작했다. 예전 개발자 워크샵때 GAN 주제로 발표하기 위해서 만들어놓은 **miniconda** env 가 있었다. 그러나 PyCharm 에 연동하는 과정에서 이상한 현상들을 발견해서 이참에 회사 머신에 **Anaconda3** 를 설치했다. [블로그 포스트를](https://uoa-eresearch.github.io/eresearch-cookbook/recipe/2014/11/20/conda/) 참고해서 간단하게 conda env 를 만들었다. `README.md` 을 따라서 `python train.py` 를 실행하고 **tensorboard** 명령어를 실행하고나니 브라우저에서 iteration 별 loss 등을 조회할 수 있었다.
* 계속 트레이닝을 시키니 loss 값이 [5.0 근방에 수렴해버리는걸](https://drive.google.com/file/d/0B7ze1RzdjqEIYnRTWHE1VXEwNFE/view?usp=sharing) 확인할 수 있었다. 모델을 저장하고, 다시 로드해서 sampling 해보니 제대로 학습되지 못한걸 확인할 수 있었다. `완전히 박살나서` 로 prime 을 줬을때 `완전히 박살나서여도대각괴 배다,얼%않에려강르서 ,에서무하 P표었 개안령한 밍멀 연 간분후트는'중높간마 욱...` 으로 이상한 문장을 내뱉는다. loss 가 5.0 근방에 수렴하는 원인을 분석하는게 최우선이라는 생각이 들었다.

### 2017-09-10

* 스터디 동료분이 주말동안 **min-char-rnn** 을 나무위키 데이터로 학습시켜봤는데, 데이터가 GB 수준으로 커지니 sampling 이 엉망이었다. 단일 레이어 RNN 의 한계라고 판단했다. **min-char-rnn** 의 업그레이드 버전으로 (마찬가지로 Andrej Karpathy 의) [char-rnn repo](https://github.com/karpathy/char-rnn) 가 있었다. 단일 레이어를 멀티 레이어로, RNN 을 LSTM/GRU 로 확장한 프로젝트였다.
* Andrej Karpathy 의 char-rnn 을 다음 단계 목표로 삼으려고 했으나, Lua 기반의 Torch 프레임워크를 사용한 프로젝트였다. Lua 언어가 Python 과 비슷하다고는 들었지만 아무래도 사용해본 적 없는 언어고, 딥러닝 프레임워크를 앞으로 익힌다면 Python 기반의 프레임워크를 익혀야겠다고 생각하고 있었다. 혹시나 싶어 찾아보니 char-rnn 프로젝트를 Tensorflow 로 다시 포팅한 [char-rnn-tensorflow](https://github.com/sherjilozair/char-rnn-tensorflow) 프로젝트가 있었다. [Andrej Karpathy 본인이 트윗에서도 언급한적이 있고](https://twitter.com/karpathy/status/670669095197614082), star 가 1,500 여개로 많아서 믿을만한 프로젝트라고 판단했다.

### 2017-09-08

* cs224n 강좌를 완강하고, 간단한 프로젝트로 character 기반의 language model 을 학습시켜보기로 했다. 예전에 cs231n 수업때 Andrej Karpathy 가 RNN 내용을 설명하면서 자신의 [min-char-rnn Gist](https://gist.github.com/karpathy/d4dee566867f8291f086) 를 소개했던게 생각났다. 간단한 python 파일 하나로 구성된 단일 레이어 RNN 이었다.
* 한글이 적힌 간단한 **input.txt** 파일을 open 하고 read 하려는데, `UnicodeDecodeError: 'cp949' codec can't decode byte 0xeb in position 6: illegal multibyte sequence` 에러가 발생했다. `encoding` keyword parameter 를 명시적으로 `"utf-8"` 로 넘기니까 해결됬다.
* 네이버 메인 기사중 임의로 하나 골라서 본문을 **input.txt** 로 넘기고 학습시켜봤다. 데이터가 작아서 빠른 속도로 loss 가 줄어들었다. 사드 관련 기사라서 대통령이 많이 언급됬었는데, `문` 을 시작 character 로 넣고 sampling 해보니 `문재인 대통령은...` 하면서 기사 본문의 내용을 잘 내뱉기 시작했다.

### 2017-01-30

* tiny imagenet 데이터를 로딩하려고 했는데 `MemoryError` 가 던져졌다. [Python 32-bit memory limits on 64bit windows](http://stackoverflow.com/questions/18282867/python-32-bit-memory-limits-on-64bit-windows) 라는 스택오버플로우 글을 읽어보니, Windows 7 기준으로 32-bit process 가 점유할 수 있는 메모리 한계가 2 GB 였다. tiny imagenet 데이터가 2.9 GB 로 더 커서 로딩할 수 없었던 거였다. 따라서 32-bit python 으로는 불가능하다고 판단했다.
* **Anaconda3 64-bit** 을 GUI 로 설치하고나니 드디어 `ImageGradients.ipynb` 에서 cython build 문제와 `MemoryError` 문제를 모두 피할 수 있었다. Python 2 의 용법을 Python 3 에 맞게 바꾸는 과정이 필요했다. 하지만 결국 temp 파일에 대한 **WinError 32** 는 피하지 못했다.

```
PermissionError: [WinError 32] 다른 프로세스가 파일을 사용 중이기 때문에 프로세스가 액세스 할 수 없습니다: 'C:\\Users\\User\\AppData\\Local\\Temp\\tmp5uphpy64' 
```

### 2017-01-27

* **RNN Captioning** 과제를 마무리 하면서, 과제 skeleton code 가 참 구성이 잘 되어있다는 생각을 했다. 별로 머리를 많이 안써도, skeleton code 의 요소들을 building block 으로 아구만 잘 끼워맞추면, 코드가 동작한다. 마지막 sampling 파트는 cache 파일 겹치는 고질적인 에러때문에 끝마치지 못했다.
* Anaconda 가 사실상 별 역할을 하지 않으면서 혼란만 가중시킨다는 생각이 들었다. 그래서 `pip` 를 활용해서 커맨드 라인에서 의존 라이브러리들을 직접 설치해보려고 시도했다. `gnureadline` 패키지에서 발생한 에러는, 패키지 사이트의 내용을 읽어보고 Windows 사용자를 위한 `pyreadline` 패키지로 대체해서 해결했다. `scipy` 패키지에서도 비슷한 문제가 발생했다. [scipy 홈페이지](https://www.scipy.org/install.html#individual-packages) 의 Windows package 항목을 보니 Windows 에서는 일반적인 방법으로 다운로드 할 수 없어보여서, 페이지에서 제안한 대안을 사용했다. 바이너리를 .whl 파일로 다운로드하고, pip 로 이를 직접 설치한다. 이때 64bit 라고 amd64 붙은 파일을 쓰면 안되고, win32 파일을 사용해야한다.
* 신경써줘야 할 것들이 계속 발생해서 결국 포기했다. 대신 IPython notebook 을 실행할때 Anaconda GUI 를 사용하지 않고, 직접 conda env 활성화하고, CLI 커맨드로 실행한다.