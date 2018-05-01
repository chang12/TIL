outlier 가 존재할때, 데이터의 통계값을 어떻게 표현하는게 좋을지 고민하다가, 동료분의 조언을 듣고 [Box plot](https://en.wikipedia.org/wiki/Box_plot) 을 찾아봤다. 보통 box plot 을 그릴때, box 에 더하여 outlier 들을 표시하기 위한 선도 긋는데, 이를 통틀어서 **box-and-whisker plot** 라고 부른단다. box-and-whisker plot 을 이해하기 위해 [Quartile](https://en.wikipedia.org/wiki/Quartile) 에 대해서도 찾아봤는데, 흔히 우리가 "사분위수" 라고 부르는 개념이었다. 이들 개념을 종합한, 가장 기본적인 형태의 box-and-whisker plot 는 아래 과정으로 그린다.

* Q1, Q3 로 box 를 그린다.
* Q2 로 box 안에 band 를 그린다.
* Q1 아래, Q3 위로 Interquartile Range (IQR) x 1.5 만큼 선을 긋는다. (whisker)

![box-and-whisker plot from wikipedia](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Michelsonmorley-boxplot.svg/600px-Michelsonmorley-boxplot.svg.png)