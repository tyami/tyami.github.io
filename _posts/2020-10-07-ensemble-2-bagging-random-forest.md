---
title: "배깅 앙상블 (Bagging Ensemble): Random Forest"
excerpt: "배깅 앙상블의 예시인 Random Forest를 정리해봅시다"

categories:
- Machine learning

tags:
- Machine learning
- Ensemble
- Algorithm
- Bagging

toc: true
toc_sticky: true
toc_label: "Random Forest"

use_math: true
---

이전 글 보기: [앙상블 (Ensemble)의 기본 개념](https://tyami.github.io/machine%20learning/ensemble-1-basics/)

> 이전 포스팅에서는 앙상블 (Ensemble)의 기본적인 개념과 그 종류들에 대해 정리했습니다.  
> 이번 포스팅에서는 그 중 배깅 (Bagging) 앙상블의 대표적인 예시인 Random Forest 알고리즘에 대해 정리하겠습니다. 정말 간단합니다.
 
## Random Forest

![숲](https://upload.wikimedia.org/wikipedia/commons/7/77/Latvian_Forest_Tomes_pagasts%2C_%C4%B6eguma_novads%2C_Latvia.jpg)

> **숲**에는 많은 **나무**들이 있고, 이 **나무**들은 서로 다른 가지의 개수, 모양, 형태를 가집니다.

이 문장에서 숲을 Random Forest로, 나무를 Decision tree로 바꾸어봅시다.

> **Random Forest**에는 많은 **Decision tree**들이 있고, 이 **Decision tree**들은 서로 다른 가지의 개수, 모양, 형태를 가집니다.

![Random Forest overview]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-07-random-forest-overview.png)

Random Forest는 배깅 (Bagging) 앙상블 알고리즘의 대표적인 예시입니다. Bagging이란 Bootstrap aggregating의 줄임말로, 이름 그대로 Bootstrap 기반의 앙상블 알고리즘입니다.

Random forest의 생성 과정을 세 개의 파트로 나누어 정리했는데, 앞의 포스팅들에서 다 설명한 내용들이라 간단히 설명하겠습니다.

- [Bootstrap](https://tyami.github.io/machine%20learning/ensemble-1-basics/#%EB%B0%B0%EA%B9%85-bagging)
- [Decision tree](https://tyami.github.io/machine%20learning/decision-tree-1-concept/)
- [Ensemble](https://tyami.github.io/machine%20learning/ensemble-1-basics/)

---

### Bootstrap

![Random Forest Bootstrap]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-07-random-forest-bootstrap.png)

주어진 데이터셋으로부터 Random sampling을 통해 각 decision tree를 만들기 위한 subset을 생성합니다. 이 때, sampling되는 데이터는 중복을 허용합니다.

---

### Decision tree

![Random Forest Decision tree]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-07-random-forest-decision-tree.png)

Bootstrap을 통해 생성된 각각의 데이터셋에 대한 Decision tree들을 구성합니다.

---

### Ensemble

![Random Forest Decision tree]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-07-random-forest-ensemble.png)

각 Decision tree의 예측 결과를 voting하여 최종 예측값을 얻습니다. 

## Python code
python `scikit-learn` 라이브러리의 `sklearn.ensemble.RandomForestClassifier` 또는 `sklearn.ensemble.RandomForestRegressor`를 이용해 Random Forest를 사용할 수 있습니다. 

- [sklearn.ensemble.RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier)
- [sklearn.ensemble.RandomForestRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor)

```python
# library load
from sklearn.ensemble import RandomForestRegressor

# build model
mdl = RandomForestRegressor()

# fit (training)
mdl.fit(X_trn, y_trn)

# predict (testing)
mdl.predict(X_tst, y_tst)
```

---

> 다음 포스팅부터는 부스팅 (Boosting) 앙상블의 초기 모델인 AdaBoost 알고리즘에 대해 정리해보겠습니다.

다음 글 보기: [부스팅 앙상블 (Boosting Ensemble) 1: AdaBoost](https://tyami.github.io/machine%20learning/ensemble-3-boosting-AdaBoost/)