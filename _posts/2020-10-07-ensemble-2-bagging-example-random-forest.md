---
title: "배깅 앙상블(Ensemble): Random Forest"
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

> **Random Forest**는 많은 **Decision tree**의 집합입니다. 이 **Decision tree**들은 서로 다른 가지의 개수, 모양, 형태를 가집니다.

![Random Forest overview]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-07-random-forest-overview.png)

Random Forest는 배깅 앙상블 알고리즘의 대표적인 예시입니다. 세 개의 파트로 나누어 정리했는데, 앞의 포스팅들에서 다 설명한 내용들이라 간단히 설명하겠습니다.

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

Bootstrap을 통해 생성된 데이터셋으로부터 Decision tree를 구성합니다.

---

### Ensemble

![Random Forest Decision tree]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-07-random-forest-ensemble.png)

Decision tree의 예측값들을 앙상블하여 최종 예측값을 얻습니다. 

---

> 다음 포스팅부터는 부스팅 (Boosting) 앙상블에 대해 정리해보겠습니다.