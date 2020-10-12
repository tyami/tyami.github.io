---
title: "부스팅 앙상블 (Boosting Ensemble) 3-1: XGBoost for Regression"
excerpt: "Boosting 모델 중 하나인 XGBoost의 Regression 알고리즘을 정리해봅시다"

categories:
- Machine learning

tags:
- Machine learning
- Ensemble
- Algorithm
- Boosting

toc: true
toc_sticky: true
toc_label: "XGBoost for Regression"

use_math: true
---

이전 글 보기: [부스팅 앙상블 (Boosting Ensemble) 2-1: Gradient Boosting for Classification](https://tyami.github.io/machine%20learning/ensemble-5-boosting-gradient-boosting-classification/)

> 이전 두 개의 포스팅에서 부스팅 앙상블의 초기 모델인 Gradient Boosting의 두 알고리즘 (Regression, Classification)에 대해 정리했습니다.  
> 이번 포스팅에서는 최근 Kaggle에서 높은 점수를 기록하고 있는 XGBoost 알고리즘에 대해 정리해보겠습니다. Regression과 Classification 중 Regression 알고리즘을 먼저 다뤄봅니다.
 
## XGBoost 
XGBoost는 2016년 Tianqi Chen과 Carlos Guestrin 가 [XGBoost: A Scalable Tree Boosting System](https://arxiv.org/abs/1603.02754) 라는 논문으로 발표했으며, 그 전부터 Kaggle에서 놀라운 성능을 보이며 알려졌습니다.

![XGBoost 자랑]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-13-xgboost-introduction-kaggle-winners.png)

XGBoost의 특징을 요약하면 아래와 같습니다.

1. Gradient Boost
2. Regularization (\\(\lambda)\\)
3. An unique regression tree (\\(\gamma)\\)
4. Approximate Greedy Algorithm
5. Parallel learning
6. Weighted Quantile Sketch
7. Sparsity-Aware Split Finiding
8. Cache-Aware Access
9. Blocks for Out-of-Core Computation

이 중 앞의 세 항목은 XGBoost의 핵심 컨셉을 나타내고, 4-9번 항목은 알고리즘 효율성을 위한 최적화 방법을 나타내는 특징입니다. 4-9 번 항목은 [XGBoost Part 4: Crazy Cool Optimizations](https://www.youtube.com/watch?v=oRrKeUCEbq8)를 참고합시다 (추후 업로드 예정)

## XGBoost for Regression

1.
2.
3.

### 1. 

### 2. 

### 3. 

## Python code
Python 에서는 `xgboost` library로 사용 가능합니다.

- [XGBoost Docs](https://xgboost.readthedocs.io/en/latest/)

```python

```

---

> 다음 포스팅에서는 XGBoost 모델의 Classification 알고리즘을 정리해보도록 하겠습니다.

다음 글 보기: [부스팅 앙상블 (Boosting Ensemble) 3: XGBoost for Classification](https://tyami.github.io/machine%20learning/ensemble-7-boosting-XGBoost-classification/)