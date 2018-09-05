[Spark Documentation](https://spark.apache.org/docs/2.2.0/index.html) 상단바에서 Programming Guides 를 클릭하면 목록에 MLlib (Machine Learning) 항목이 있다. 클릭해서 들어가면 왼쪽 목차에 **Extracting, transforming and selecting features** 항목이 있다. 들어가보면 또 목차가 보이는데 그중에 [Feature Selectors](https://spark.apache.org/docs/2.2.0/ml-features.html#feature-selectors) 항목이 있다. 18.03.15 기준으로 3가지 feature selector 가 제공된다.

# Vector Slicer

> VectorSlicer is a transformer that takes a feature vector and outputs a new feature vector with a sub-array of the original features. It is useful for extracting features from a vector column.

말 그대로 slicer 해주는 녀석이다.

# RFormula

> RFormula selects columns specified by an R model formula. 

R 의 개념을 가져왔나보다. feature 가 `String` 이라면 one-hot encoding 하는등의 작업이 알아서 이뤄진다.

# ChiSqSelector

> ChiSqSelector stands for Chi-Squared feature selection. It operates on labeled data with categorical features. ChiSqSelector uses the Chi-Squared test of independence to decide which features to choose. It supports five selection methods: numTopFeatures, percentile, fpr, fdr, fwe... (후략)

뭔가 관심이 가는 내용이 등장했다. 5가지 selection methods 를 제공한다는데 읽어보니 앞의 2개만 알겠고 나머지는 p-value 등이 등장하면서 모르겠다. 

> numTopFeatures chooses a fixed number of top features according to a chi-squared test. This is akin to yielding the features with the most predictive power. * percentile is similar to numTopFeatures but chooses a fraction of all features instead of a fixed number.

chi-squared test 를 시행하면 feature 의 predictive power 를 알 수 있나보다!
