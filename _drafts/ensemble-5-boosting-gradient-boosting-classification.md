---
title: "부스팅 앙상블 (Boosting Ensemble) 2-1: Gradient Boosting for Classification"
excerpt: "Boosting 알고리즘 중 하나인 Gradient Boosting for Classification을 정리해봅시다"

categories:
- Machine learning

tags:
- Machine learning
- Ensemble
- Algorithm
- Boosting

toc: true
toc_sticky: true
toc_label: "Gradient Boosting for Classification"

use_math: true
---

이전 글 보기: [부스팅 앙상블 (Boosting Ensemble) 2-1: Gradient Boosting for Regression](https://tyami.github.io/machine%20learning/ensemble-4-boosting-gradient-boosting-regression/)

> 이전 포스팅에서는 Gradient Boosting 알고리즘 중 Regression 알고리즘을 정리했습니다.  
> 이번 포스팅에서는 Gradient Boosting for Classfication 알고리즘을 정리해보고자 합니다.

Gradient Boosting for Regression과 마찬가지로, StatQuest라는 유투버의 [Gradient Boost Part 3: Classification](https://www.youtube.com/watch?v=jxuNLH5dXCs)과 [Gradient Boost Part 4: Classification Details](https://www.youtube.com/watch?v=StWY5QWMXCw)를 참고했습니다.
 
## Gradient Boosting for Classification

Gradient Boosting for Classification은 Logistic regression과 많이 비슷합니다.

1. Create a first leaf

초기 값은 logit \\(log(odds)\\)를 사용합니다.

Odds는 **임의의 사건 A가 발생하지 않을 확률 대비 일어날 확률의 비율**입니다. 보다 구체적인 설명은 [ratsgo's blog](https://ratsgo.github.io/machine%20learning/2017/04/02/logistic/)를 참고합시다 (언젠가 내 워딩으로 포스팅하기)

\[
odds=\frac{P(A)}{P(A^c)}=\frac{P(A)}{1-P(A)}
\]

odds는 P(A)가 0에 가까울수록 0 값을 가지며, 1에 가까울수록 커지면서 무한대로 발산합니다.  
여기에 log 변환을 해준 logit은 0.5를 대칭으로 -무한대에서 무한대로 발산합니다.

2. Calculate ~
3. Calculate
4. Create next tree
5. Repeat 2-4


![Caption]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/파일명.png)

---

> 다음 포스팅에서는 **드디어** Kaggle에서 많이 사용되고 있는 대표적인 부스팅 앙상블 알고리즘인 XGBoost를 정리해보고자 합니다.

다음 글 보기: [부스팅 앙상블 (Boosting Ensemble) 3: XGBoost](https://tyami.github.io/machine%20learning/ensemble-6-boosting-gradient-boosting-classification)
