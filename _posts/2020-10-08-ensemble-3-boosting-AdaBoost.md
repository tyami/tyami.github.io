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

> 이전 포스팅에서는 앙상블 (Ensemble)의 기본적인 개념과 그 종류들에 대해 정리했습니다.  
> 이전 포스팅에서는 배깅 (Bagging) 앙상블의 대표적인 예시인 Random Forest 알고리즘에 대해 정리했습니다.
> 이번 포스팅에서는 또 다른 앙상블 알고리즘인 부스팅 알고리즘의 가장 첫번째 모델인 AdaBoost 알고리즘을 정리해보겠습니다.
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
### 1. Initialize sample weight
> 핵심: initial sample weight는 모두 동일하게 해준다

\[ 
sample\; weight=1/total\; number\; of\; samples
\]

### 2. Complete first stump
Impurity를 바탕으로 best attribute로 분기하는 stump를 완성한다.

### 3. Calculate amount of say
완성한 stump의 분류 결과에 따라 amount of say (stump의 weight)

\\(total error\\)는 sum of the sample weights associated with the incorrectly classified samples로 정의될 수 있다. Sample weights의 총합이 1이기 때문에 최고의 Stump (모두 맞춤)를 만든 경우 total error 는 0, 반대로 최악의 Stump를 만든 경우 total error는 1을 갖는다.

\[
Amount\; of\; Say=\frac{1}{2}log(\frac{1-total\; error}{total\; error})
\]

### 4. Sample weight (update)
핵심: 오분류된 샘플의 sample weight는 늘리고, 잘 분류된 샘플의 sample weight는 낮춘다.

#### 4.1. 오분류된 샘플의 sample weight 늘리기
\[
New\; sample\; weight=sample\; weight \times e^{amount\; of\; say}
\]

#### 4.2. 잘 분류된 샘플의 sample weight 낮추기
\[
New\; sample\; weight=sample\; weight \times e^{-amount\; of\; say}
\]

#### 4.3 sample weight의 총합이 1이 되도록 normalize하기
\[
Normalized\; sample\; weight=\frac{New\; sample\; weight}{\sum New\; sample\; weight}
\]

### 5. Complete next stump
- impurity 계산할 때 두 가지 방법으로 sample weight를 적용할 수 있다.

#### 5.1. **Weighted Gini index \\(WG(S)\\)**

\begin{aligned}
G(S) &= \sum_{i=1}^C p_i(1-p_i) \\\\\\
&= \sum_{1=1}^C p_i - \sum_{1=1}^C p_i^2 \\\\\\
&= 1 - \sum_{i=1}^Cp_i^2 \\\\\\
\end{aligned}

\[
WG(S) = \sum_{i=1}^C w_ip_i(1-p_i)
\]

#### 5.2. 중복을 허용하여 **Dataset resampling** 후 Gini index 이용
- Sample weight를 바탕으로 0~1의 구간을 나눠준다. 
- 0~1값을 원래 샘플 수만큼 random sampling 하고, 이 값에 따라 원 데이터에서 데이터를 다시 sampling한다. 
- Sample weight가 큰 경우, 더 넓은 구간을 갖기 때문에, 이 샘플이 더 자주 뽑히게 된다.
  
- 이 방법을 사용할 경우, 새로 sampling한 데이터셋의 sample weight는 \\(1/total\; number\; of\; samples\\)로 초기화해준다. sample weight는 일정하지만, 동일한 샘플이 더 많이 있기 때문에 sample weight이 다른 효과를 줄 수 있다.

### 6. 정해진 iteration만큼 stump가 생성될 때까지 4~5 과정을 반복한다.

### 7. 테스트 시에는, 모든 Stump 의 예측값에 amount of say를 가중치로 주고, Hard voting을 해서 최종 예측값을 얻어낸다.