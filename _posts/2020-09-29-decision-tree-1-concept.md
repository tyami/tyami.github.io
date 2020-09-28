---
title: "의사결정 나무 (Decision Tree) 기본 설명"
excerpt: ""

categories:
- Machine learning

tags:
- Machine learning
- Decision Tree
- Algorithm
---

이 블로그에서의 첫 글이네요 !

## Motivation
본 포스팅에서는 기계 학습 (Machine learning)의 대표적인 알고리즘 중 하나인 의사결정 나무 (Decision tree) 알고리즘에 대해서 다뤄보고자 합니다.

개념이 워낙 간단한 알고리즘이고, 최근 들어 워낙 심층 학습 (Deep learning)이 대세로 굳어지다보니 '이걸 굳이 자세히 공부해야하나?' 라는 생각을 갖고 있던 알고리즘이었는데, 최근 Kaggle과 같은 데이터 분석 대회에 관심을 갖게 되면서 공부할 필요성을 느끼게 되었습니다.

![2019년 기준 Kaggle competition에서 많이 사용된 알고리즘 2, 3위가 Decision tree 기반 앙상블 기법입니다](https://pbs.twimg.com/media/D3Pb_Q3UIAAuSWU?format=jpg&name=medium)  
2019년 4월, Keras library의 개발자 중 한 명인 François Chollet의 [tweet](https://twitter.com/fchollet/status/1113476428249464833?lang=en)에 올라온 내용입니다.
Kaggle competition 상위권 팀들의 알고리즘을 종합해본 결과, 2, 3위를 딥러닝 기반이 아닌 Gradient Boosting Decision Tree (GBDT) 기반 Ensemble 모델이 차지했습니다.

따라서 본 포스팅으로 시작되는 일련의 포스팅들에서는 Decision Tree의 기초 알고리즘부터 시작해서 XGBoost, LightBoost, CatBoost와 같은 GBDT 기반 모델들까지 정리해보고자 합니다.

## 목차
- Decision Tree 의 기본 개념
- Decision Tree 알고리즘 1: ID3
- Decision Tree 알고리즘 2: C4.5 
- Decision Tree 알고리즘 3: CART (Classification And Regression Tree)
- Ensemble의 기본 개념: Bagging과 Boosting
- Bagging Ensemble 예시 1: Random Forest
- Boosting Ensemble 예시 2: XGBoost
- Boosting Ensemble 예시 3: LightGBM
- Boosting Ensemble 예시 4: CatBoost
 
 ## Decision Tree 의 기본 개념

Decision Tree는 **일련의 필터 과정** 또는 **스무고개**라고 생각하면 됩니다.


아래와 같은 데이터가 있다고 생각해봅시다.  
Outlook, Temperature, Humidity, Windy와 같은 기상조건에 따라 운동을 할지 말지 (Play) 결정하는 모델을 만들고자 합니다.

| Outlook  | Temperature | Humidity | Windy | Play |
|:--------:|:-----------:|:--------:|:-----:|:----:|
| sunny    | hot         | high     | FALSE | No   |
| sunny    | hot         | high     | TRUE  | No   |
| overcast | hot         | high     | FALSE | Yes  |
| rain     | mild        | high     | FALSE | Yes  |
| rain     | cool        | normal   | FALSE | Yes  |
| rain     | cool        | normal   | TRUE  | No   |
| overcast | cool        | normal   | TRUE  | Yes  |
| sunny    | mild        | high     | FALSE | No   |
| sunny    | cool        | normal   | FALSE | Yes  |
| rain     | mild        | normal   | FALSE | Yes  |
| sunny    | mild        | normal   | TRUE  | Yes  |
| overcast | mild        | high     | TRUE  | Yes  |
| overcast | hot         | normal   | FALSE | Yes  |
| rain     | mild        | high     | TRUE  | No   |

예를 들면, 이러한 Decision Tree를 만들 수 있습니다.
[decision-tree-example-1]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/decision-tree-example-1.png)


