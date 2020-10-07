---
title: "부스팅 앙상블(Ensemble) 1: AdaBoost"
excerpt: "부스팅 앙상블의 예시인 AdaBoost를 정리해봅시다"

categories:
- Machine learning

tags:
- Machine learning
- Ensemble
- Algorithm
- Boosting

toc: true
toc_sticky: true
toc_label: "AdaBoost"

use_math: true
---

이전 글 보기: [배깅 앙상블(Ensemble): Random Forest](https://tyami.github.io/machine%20learning/ensemble-2-bagging-example-random-forest/)

> 이전 포스팅에서는 배깅 (Bagging) 앙상블의 대표적인 예시인 Random Forest 알고리즘에 대해 정리했습니다.
> 이번 포스팅에서는 또 다른 앙상블 알고리즘인 부스팅 알고리즘의 가장 첫번째 모델인 AdaBoost 알고리즘을 정리해보겠습니다. 그림은 나중에 추가하는거로 !

- 참고 자료 (Youtube): [AdaBoost, Clearly Explained](https://www.youtube.com/watch?v=LsK-xG1cLYA) by StatQuest

## AdaBoost의 3가지 Concepts
1. Forest of stumps
2. Different weights
3. Sequential

### 1. Forest of stumps
- Random forest에서는 개별 모델로 **decision tree**를 사용한다.
- AdaBoost에서는 개별 모델로 **stump**를 사용한다.
  - Stump: a tree with just one node and two leaves
  - Stump는 tree보다 정확도는 떨어지는 weak learner다.

### 2. Different weights for each stump 
- Random forest에서 모든 tree는 **동일한 weight**를 갖는다.
- AdaBoost에서 각 stump는 **서로 다른 weight (amount of say)**를 갖는다.

### 3. Sequential
- Random forest에서 tree들은 **parallel**하게 만들어진다.
- AdaBoost에서 stump들은 **sequential**하게 만들어진다.

## AdaBoost 학습 순서
### 1. Initial sample weight
Initial sample weight는 모두 동일하게 해준다

\[ 
sample\; weight=\frac{1}{total\; number\; of\; samples}
\]

### 2. First stump
Impurity를 바탕으로 best attribute로 분기하는 stump를 완성한다.

### 3. Amount of say
완성한 stump의 분류 결과에 따라 amount of say (stump의 weight)를 계산해준다.

\\(total\; error\\)는 **오분류된 샘플의 sample weight 총합**으로 정의될 수 있다. 전체 Sample weights의 총합이 1이기 때문에 최고의 Stump (모두 맞춤)를 만든 경우 total error 는 0, 반대로 최악의 Stump를 만든 경우 total error는 1을 갖는다.

\[
Amount\; of\; say=\frac{1}{2}log(\frac{1-total\; error}{total\; error})
\]

total error에 따른 Amount of say의 변화를 그림으로 그려보면, total error가 낮을 때 (<0.5)는 양의 amount of say, total error가 높을 때 (>0.5)는 음의 amount of say를 갖는다. 또한 amount of say의 절대값은 total error가 극단적인 값일 수록 (0 또는 1에 가까울수록) 더욱 더 커진다.

### 4. Updated sample weight
오분류된 샘플의 sample weight는 늘리고, 잘 분류된 샘플의 sample weight는 낮춘다.  

#### 4.1. 오분류된 샘플
샘플마다 업데이트되는 가중치 양에는 차이가 없다.

\[
New\; sample\; weight=sample\; weight \times e^{amount\; of\; say}
\]

#### 4.2. 정분류된 샘플
마찬가지로 샘플마다 업데이트되는 가중치 양에는 차이가 없다.

\[
New\; sample\; weight=sample\; weight \times e^{-amount\; of\; say}
\]

#### 4.3 총합=1로 정규화
Sample weight의 총합이 1이 되도록 normalize해준다.

\[
Normalized\; sample\; weight=\frac{New\; sample\; weight}{\sum New\; sample\; weight}
\]

### 5. Next stump
Sample weight를 적용한 새로운 Stump를 만든다. 분기 시, Impurity를 계산할 때 두 가지 방법으로 sample weight를 적용할 수 있다.

#### 5.1. Weighted Gini index

\[
Weighted\; Gini\; index= \sum_{i=1}^C w_ip_i(1-p_i)
\]

#### 5.2. Resampling
중복을 허용하여 Dataset을 resampling 후 Gini index를 계산한다
- Sample weight를 바탕으로 0~1의 구간을 나눠준다. 
- 0~1값을 원래 샘플 수만큼 random sampling 하고, 이 값에 따라 원 데이터에서 데이터를 다시 sampling한다. 
- Sample weight가 큰 경우, 더 넓은 구간을 갖기 때문에, 이 샘플이 더 자주 뽑히게 된다.
이 방법을 사용할 경우, 새로 sampling한 데이터셋의 sample weight는 \\(\frac{1}{total\; number\; of\; samples}\\)로 초기화해준다. sample weight는 일정하지만, 동일한 샘플이 더 많이 있기 때문에, 결과적으로 sample weight이 다른 것과 동일한 효과를 줄 수 있다.

### 6. Repetition
정해진 iteration만큼 stump가 생성될 때까지 3~5 과정을 반복한다.

### 7. 최종 예측
테스트 시, 모든 Stump 의 예측값에 amount of say를 가중치로 주고, Hard voting을 해서 최종 예측값을 얻어낸다.