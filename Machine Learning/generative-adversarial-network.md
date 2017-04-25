GAN 을 공부해보고 싶었다. 스탠포드 CS231n 수업에서 잠깐 내용을 들었던 기억이 났지만, 그새 다 까먹었는지 기억이 나지 않았다. 계기는 개발팀 워크샵의 라이트닝 토크 준비였다.

## Keras 로 구현한 프로젝트 탐색 -> 포기

* GitHub 의 [KerasGAN](https://github.com/osh/KerasGAN) 프로젝트를 탐색했다. 구글링했을때 가장 먼저 나오고, `.py` 파일 하나로 구현도 간단하고, GitHub Star 도 230여개로 꽤나 많았어서 괜찮아보였다. 하지만 코드를 탐색해보니 Keras 1.0 API 로 구현되어있었고, 1.0 API 를 2.0 API 로 어떻게 바꾸는지 구글링으로 못 찾겠어서 제꼈다.
* 그 다음으로 GitHub 의 [keras-adversarial](https://github.com/bstriner/keras-adversarial) 프로젝트를 탐색했다. 가장 최근 커밋이 9일 전으로 관리가 잘 되고있는 것 같아 보였다. 또한 단순히 구현만 한게 아니라 다른 사람들이 Keras 로 원하는 GAN 을 구현하도록 도와주는 라이브러리 처럼 보였다. 하지만 개발 환경을 구축하는 도중에 Fortran 컴파일러가 설치되어있지 않다면서 에러를 내뱉는거보고 또 제꼈다.

## 베이스 프로젝트 결정

* Keras 라는 제약 조건을 풀고 나서 찾아보니 GitHub 의 [generative-models](https://github.com/wiseodd/generative-models) 프로젝트를 찾을 수 있었다. 18 가지 GAN 알고리즘을 PyTorch / Tensorflow 로 구현해놓은 프로젝트였다. GitHub Star 개수도 900여개로 이전 두 프로젝트에 비해 월등히 많았다. Tensorflow 를 설치하는 것은 Keras / Theano 를 설치하는것에 비해 상대적으로 간단했다.
* 가장 간단한 vanilla GAN 을 선택해서 코드를 탐색했다. Generator / Discriminator 의 개념을 이해하는 것은 그리 어렵지 않았다. vanilla GAN 에서는 두 네트워크 모두 가장 간단한 **2-layer neural network** 로 구성되어있었기에 구조를 파악하는게 수월했다.

## 실습 1 : MNIST

* 프로젝트 코드가 MNIST 데이터를 생성하는 GAN 을 학습시키는 코드로 준비되어있다. 그러므로 개발환경만 갖추고나서 바로 학습을 돌릴 수 있었다. 100 iterations/sec 정도 소요되는데 80,000 iteration 정도만 학습시켜도 그럴싸한 hand-written digit 이미지를 생성하는 것을 확인할 수 있었다.

## 실습 2 : 스티커 이미지

* 정적/동적 스티커 파일을 PNG 이미지들로 변환하는 스크립트를 짜는데 생각보다 시간이 오래 걸렸다.
* PNG 이미지와 numpy array 간의 변환은 `scipy` 패키지의 `misc` 모듈을 사용하면 간단하다. PNG 이미지의 경우 투명도를 위한 알파 채널을 가지고 있으므로, width x height x 4 dimension 의 numpy array 로 만들어진다. 기존 GAN 코드가 [-1, +1] 범위의 noise 벡터를 가정하므로 translation / division 정도의 정규화만 끼얹었다. 