* [Standford cs231n 스터디](https://github.com/Industrial-Functional-Agent/cs231n) with @bellatoris

## 타임라인

### 2017-01-27 (FRI)

* **RNN Captioning 마무리** : 과제 skeleton code 가 참 구성이 잘 되어있다는 생각을 했다. 별로 머리를 많이 안써도, skeleton code 의 요소들을 building block 으로 아구만 잘 끼워맞추면, 코드가 동작한다. 마지막 sampling 파트는 cache 파일 겹치는 고질적인 에러때문에 끝마치지 못했다.
* **Anaconda 대신 CLI 로 환경 설치** : Anaconda 가 사실상 별 역할을 하지 않으면서 혼란만 가중시킨다는 생각이 들었다.
    * `pip install -r requirements.txt` 로 패키지를 설치하는 도중에 `gnureadline` 패키지에서 에러가 발생했다. 패키지 사이트의 내용을 읽어보니 Windows 사용자는 `pyreadline` 패키지를 사용하라는 가이드가 있었다. requirements.txt 의 내용을 교체했다.
    * **scipy** 패키지에서도 비슷한 문제가 발생했다. [scipy 홈페이지](https://www.scipy.org/install.html#individual-packages) 의 Windows package 항목을 보니 Windows 에서는 일반적인 방법으로 다운로드 할 수 없어보여서, 페이지에서 제안한 대안을 사용했다. 바이너리를 .whl 파일로 다운로드하고, pip 로 이를 직접 설치한다. 이때 64bit 라고 amd64 붙은 파일을 쓰면 안되고, win32 파일을 사용해야한다.
    * notebook 에서 추가로 필요로하는 패키지들을 설치해줘야 한다. Anaconda 를 쓰던 시절에는 Anaconda 에 모두 포함되어있던 패키지들인데, 이제 CLI 에서 설치하다보니 requirements.txt 에는 없던 애들을 신경써줘야한다.
    * 신경써줘야 할 것들이 계속 발생해서 포기한다. 단순히 `pip install -r requirements.txt` 로 해결되는 문제들이 아니라는게 짜증난다. 이래서 Anaconda 라는 덩어리를 설치하나보다.
    * 대신 IPython notebook 을 실행할때 Anaconda GUI 를 사용하지 않고, 직접 conda env 활성화하고, CLI 커맨드로 실행한다.

### 2017-01-30 (MON)

* **Anaconda2 32-bit 시도**
    * cython build 커맨드 실행하라는 메시지는 더 이상 뜨지 않는다.
    * tiny imagenet 데이터를 로딩하지 못하고, MemoryError 를 나타낸다.
    * [Python 32-bit memory limits on 64bit windows](http://stackoverflow.com/questions/18282867/python-32-bit-memory-limits-on-64bit-windows) 라는 스택오버플로우 글을 읽어보니, Windows 7 기준으로 32-bit process 가 점유할 수 있는 메모리 한계가 2 GB 였다. tiny imagenet 데이터가 2.9 GB 를 필요하다고 했으므로 로딩할 수 없는 상황이었던 것.
    * 즉, 32-bit python 으로는 불가능하다.
* **Anaconda3 64-bit 을 GUI 로 설치**
    * ImageGradients.ipynb 에서 처음으로 cython build 문제와 MemoryError 문제를 모두 피할 수 있었다. cython build 경고 메시지는 떴지만, 시키는대로 커맨드를 실행하니 뭔가 진행되는 듯한 로그를 보여주고나서 정상적으로 완료되었다.
    * Python 2 의 용법을 Python 3 에 맞게 바꾸기 위한 몇차례 수정이 있었다.
* 아직도 temp 파일에 대한 **WinError 32** 는 피하지 못하고 있다.
    * > PermissionError: [WinError 32] 다른 프로세스가 파일을 사용 중이기 때문에 프로세스가 액세스 할 수 없습니다: 'C:\\Users\\User\\AppData\\Local\\Temp\\tmp5uphpy64' 