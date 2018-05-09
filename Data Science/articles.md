# [Bayesian Linear Regression Part 1 - EDA, Feature Selection, and Benchmarks](https://towardsdatascience.com/bayesian-linear-regression-in-python-using-machine-learning-to-predict-student-grades-part-1-7d0ad817fca5)

## Introduction

* grade 는 nominal variable (integer) 이지만 regression 을 위해 continuous variable 로 취급한다.
* [What is the difference between categorical, ordinal and interval variables?](https://stats.idre.ucla.edu/other/mult-pkg/whatstat/what-is-the-difference-between-categorical-ordinal-and-interval-variables/)
* regression 의 목표가 되는 grade 를 target variable 혹은 response 라고 부른다.

## EDA

* grade 의 histogram 을 그려본다.
* categorical variable 로 나눠지는 group 별로 grade 의 density plot 을 그려본다.
	* group 별로 크기가 다르므로, histogram 이 아니라 정규화해서 density plot 을 그린다.
	* density plot 을 그릴때 [seaborn](https://seaborn.pydata.org/) library 의 kdeplot 을 사용하면 편리하다.
* "measure more rigorous", "statistics to back up our intuition"

## Feature Selection

* [feature selection wikipedia](https://en.wikipedia.org/wiki/Feature_selection)
* feature selection 이란 relevant variables 을 고르는 작업이다. linear modeling 할때는 [correlation coefficient](https://newonlinecourses.science.psu.edu/stat501/node/256/) 를 measure 로 삼는다.
* correlation 은 numerical variables 간에 구해지는 값이므로, categorial variable 에는 [one-hot encoding](https://hackernoon.com/what-is-one-hot-encoding-why-and-when-do-you-have-to-use-it-e3c6186d008f) 을 가한다. pivot 이랑 유사하다.
* pandas 를 쓰면 one-hot encoding 이 간단하다.
* 나는 categorical variable 을 다룰때 correlation coefficient 대신 information gain 을 계산해야한다고 생각했었다.
* (자신의 그간의 경험에 근거해서) 가장 relevant 한 variables 6개를 고른다.
* scikit-learn 의 from sklearn.model_selection import train_test_split 을 사용하면 간편하게 test / train data 를 나눌 수 있다.
* pairs plot 이라는걸 그린다. [Visualizing Data with Pairs Plots in Python](https://towardsdatascience.com/visualizing-data-with-pair-plots-in-python-f228cf529166) 글을 참고하자. seaborn 의 PairGrid function 을 사용할 수 있다.
* distribution plots 도 그린다. variable 에 따라 target variable 을 median 위/아래 2개로 나눠 plot 을 그린다.

## Establish Baselines Metrics

* regression 에서 시도할 수 있는 괜춘한 baseline 으로 "늘 median 으로 예측" 이 있다.
* Mean Absolute Error (MAE) 와 Root Mean Squared Error (RMSE) 를 metric 으로 삼는다.
* [MAE and RMSE — Which Metric is Better?](https://medium.com/human-in-a-machine-world/mae-and-rmse-which-metric-is-better-e60ac3bde13d) 라는 글에서 discussion 을 볼 수 있다.
* 궁극적인 목표는 Bayesian Linear Regression 지만, 비교 대상으로 삼기위해 다양한 methods 들을 시도해본다. scikit-learn 을 쓰면 좋다.
	* [gradient boosted](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingRegressor.html)
	* random forest
	* (ordinary least squares, ols) [linear regression](http://setosa.io/ev/ordinary-least-squares-regression/)
		* [the frequentist approach to linear modeling?](https://stats.stackexchange.com/questions/171423/is-ols-the-frequentist-approach-to-linear-regression)
		* linear regression 은 간단해서 complex / non-linear problem 에 잘 적용되지 않는 문제가 있지만, prediction formula 를 해석하기 좋다.
	* svm
	* extra trees
	* ElasticNet regression






