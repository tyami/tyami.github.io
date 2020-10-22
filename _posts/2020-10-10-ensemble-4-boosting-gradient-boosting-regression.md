---
title: "부스팅 앙상블 (Boosting Ensemble) 2-1: Gradient Boosting for Regression"
excerpt: "Boosting 알고리즘 중 하나인 Gradient Boosting for Regression을 정리해봅시다"

categories:
- Machine learning

tags:
- Machine learning
- Ensemble
- Algorithm
- Boosting

toc: true
toc_sticky: true
toc_label: "Gradient Boosting for Regression"

use_math: true
---

이전 글 보기: [부스팅 앙상블 (Boosting Ensemble) 1: AdaBoost]({{ site.url }}{{ site.baseurl }}/machine%20learning/ensemble-3-boosting-AdaBoost)

> 이전 포스팅에서는 부스팅 앙상블의 초기 모델인 AdaBoost에 대해 설명했습니다.  
> 이번 포스팅에서는 AdaBoost보다 조금 더 진보된 부스팅 앙상블 모델인 Gradient Boosting 중 Regression 알고리즘을 정리했습니다.

전체적인 내용은 StatQuest라는 유투버의 [Gradient Boost Part 1: Regression Main Ideas](https://www.youtube.com/watch?v=3CC4N4z3GJc)과 [Gradient Boost Part 2: Regression Details](https://www.youtube.com/watch?v=2xudPOBz-vs)를 참고했습니다. Gradient Boosting에 대해 가장 정리가 잘 된 설명자료입니다 (영어이지만 시각자료도 많고, 화면에 자막도 있어서 알아듣기 쉽습니다)
 
## Gradient Boosting

Gradient Boosting은 앞서 정리한 AdaBoost보다 조금 복잡합니다. 따라서 이해와 관계없이 일단 포스팅을 처음부터 끝까지 쭉 읽어서 전체적인 흐름을 이해하고, 그 다음 세부내용을 공부하는게 좋을 것 같습니다. 가능하다면 위에 링크해둔 유투브 영상도 보시는 것을 추천드립니다.

## AdaBoost VS Gradient Boosting
AdaBoost와 Gradient Boosting 두 모델의 공통점은 부스팅 앙상블 기반의 알고리즘이라는 것입니다. 부스팅 앙상블의 대표적인 특징은 모델 학습이 **sequential**합니다. 즉, 먼저 생성된 모델의 예측값이 다음 모델 생성에 영향을 줍니다.  
하지만 이 외에 두 모델은 상당한 차이점이 있습니다.

AdaBoost에 비교되는 Gradient Boosting의 대표적인 차이점은 세 가지 정도로 정리할 수 있습니다.

1. **Weak learner**: Stumps VS A leaf & Restricted trees
2. **Predicted value**: Output VS Pseudo-residual
3. **Model weight**: Different model weights (amount of say) VS Equal model weight (learning rate)

### 1. Weak learner

앙상블 모델의 기본이 되는 weak lerner가 다릅니다.

![AdaBoost VS Gradient Boost 1: weak learner]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-10-gradient-boosting-regression/2020-10-10-gradient-boosting-regression-comparison-adaboost-gradient-boost.png)

AdaBoost에서는 weak learner로 stump (한 개 노드와 두 개의 가지를 갖는 매우 작은 decision tree) 를 사용합니다.

반면 Gradient Boosting에서는 restricted tree를 사용합니다. restricted tree란, maximum number of leaves로 성장에 제한을 둔 decision tree입니다.  
또한 Gradient Boosting의 첫 번째 weak learner는 모든 샘플의 output 평균을 값으로 갖는 하나의 leaf입니다.

### 2. Predicted value

각 모델이 예측하는 정보가 다릅니다.

AdaBoost에서는 각 stump들은 모두 실제 output 값을 예측하는 모델입니다. 따라서 이 값을 평균내거나 가중치를 곱한 평균을 통해, 실제 값에 가까운 예측값을 만들어냅니다.

![AdaBoost VS Gradient Boost 2: predicted value]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-10-gradient-boosting-regression/2020-10-10-gradient-boosting-regression-test.png)

반면 Gradient Boosting에서 각 restricted tree들이 예측하는 값은 실제 output과 이전 모델의 예측치 사이의 오차 (pseudo-residual) 입니다.  
최종 예측 시에는 각 모델의 오차를 scaling 후, 합하는 과정을 통해 실제 값에 가까운 예측값을 만들어냅니다.

> Pseudo-residual에서 **Pseudo**라는 단어가 붙은 이유는 linear regression 에서의 residaul과 구별하기 위해서입니다. Gradient Boosting에서 어떤 Loss function을 사용하느냐에 따라 residual과 동일할 수도, 비슷할 수도 있기에 이런 이름을 붙였다고 합니다. ([참고](https://www.youtube.com/watch?v=2xudPOBz-vs))

### 3. Model weight

각 모델에 대해 가중치를 주는 방식이 다릅니다.

![AdaBoost VS Gradient Boost 3: model weight]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-10-gradient-boosting-regression/2020-10-10-gradient-boosting-regression-comparison-adaboost-gradient-boost.png)

다시 위 그림을 살펴보면, AdaBoost에서는 각 모델의 크기가 다른 반면, Gradient Boosting에서는 크기가 동일한 것을 알 수 있습니다.

AdaBoosting에서는 amoung of say \\(\alpha_t\\)를 인풋 \\(x\\)에 대한 각 모델의 예측값 \\(h_t(x)\\)에 곱하여 최종 예측값을 계산했습니다. 이 때 \\(\alpha_t\\)는 이전 모델의 예측결과에 따라 계산되기 때문에, \\(t\\)에 따라 상이했습니다. 따라서 \\(M\\)개 모델로 구성된 AdaBoost의 최종 예측값은 아래 수식으로 표현될 수 있습니다.

\\[
F_{t}(x)=\sum_{t=1}^M \alpha_t h_t(x)
\\]

반면 Gradient Boosting에서는 model weight로 learning rate \\(\eta\\)를 사용합니다. 이 때 \\(\eta\\)는 \\(t\\)에 관계없이 모두 동일하게 scaling합니다. 따라서 \\(M\\)개 모델로 구성된 Gradient Boosting의 최종 예측값은 아래 수식으로 표현할 수 있습니다.

\\[
F_{t}(x)=F_0(x) + \eta \sum_{t=1}^M h_t(x)
\\]

\\(F_0(x)\\)는 첫 번째 모델 (a leaf)의 값을 의미합니다.

---

## Gradiend Boosting for Regression

Gradient Boosting은 회귀 (Regression)와 분류 (Classification) 문제에 모두 사용 모두 가능합니다. 두 알고리즘은 전체적으로는 비슷하지만, 디테일 면에서 다릅니다. 알고리즘의 공통점을 요약하면 아래와 같습니다.

> Create decision trees to predict residual (observed value – predicted value) of **______**, with limitation of maximum number of leaves.

두 알고리즘은 진한 부분의 블랭크 (_____)에 무엇이 들어가느냐가 다릅니다.

본 포스팅에서는 상대적으로 쉬운 Gradient Boosting for Regression 알고리즘을 먼저 정리해보도록 하겠습니다.

![Eqations]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-10-gradient-boosting-regression/2020-10-10-gradient-boosting-regression-equations.png)

[Gradient Boost Part 2: Regression Details](https://www.youtube.com/watch?v=2xudPOBz-vs) 의 설명에 사용된 수식입니다. 이대로는 보기가 좀 어려우니, 알아 듣기 쉽도록 자연어로 다시 쓰고, 그림으로 표현해 보면 아래와 같습니다.

![Procedure overview]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-10-gradient-boosting-regression/2020-10-10-gradient-boosting-regression-procedure-overview.png)

1. Create a first leaf
2. Calculate pseudo-residuals
3. Create a next tree to predict pseudo-residuals 
4. Repeat 2-3

- (Test) Scale and add up the results of each tree

### 1. Create a first leaf

![GB step 1: create first leaf]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-10-gradient-boosting-regression/2020-10-10-gradient-boosting-regression-step1.png)

First model로 leaf를 만듭니다. 이 Leaf가 갖는 값 \\(F_0 (x)\\)은 training data의 모든 output의 평균입니다.  
초기값으로 output의 평균값을 사용하는 이유는 아래 수식을 미분해서 풀면 됩니다.

\\[
F_0 (x) = \underset{\gamma}{argmin} \sum_{i=1}^n L(y_i, \gamma)
\\]

, where Loss function \\(L(y_i, F(x))=\frac{1}{2}(Observerd-Predicted)^2\\) is a differentiable

### 2. Calculate pseudo-residuals

![GB step 2:calculate psuedo-residual]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-10-gradient-boosting-regression/2020-10-10-gradient-boosting-regression-step2.png)

Pseudo-residual (실제값 - 예측값)을 계산합니다. 

> Compute \\(r_{im}=-\frac{\partial L(y_i, F(X_i))}{\partial F(X_i)}\\), where \\(F(x)=F_{m-1}(x)\\) for \\(i=1,...,n\\)

### 3. Create a next tree to predict pseudo-residual

![GB step 3:create nex tree to predict pseudo-residual]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-10-gradient-boosting-regression/2020-10-10-gradient-boosting-regression-step3-1.png)

#### 3-1. Create a tree

주어진 데이터 (Height, Favorite color, Gender)를 바탕으로 Pseudo-residual을 예측하는 decision tree를 만듭니다. 아래와 같은 tree가 만들어집니다.

![GB step 3-1 result]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-10-gradient-boosting-regression/2020-10-10-gradient-boosting-regression-step3-1-result.png)

> Fit a regression tree to the \\(r_{im}\\) values and create terminal regions \\(R_{jm}\\), for \\(j=1,...J_m\\)

\\(R_{jm}\\)은 decision tree의 \\(j\\)번째 terminal node 내 values로 이루어진 집합을 의미합니다 (Step 3-2를 위해 생성). 위 예시에서 \\(R_{1m}\\)는 {-14.2, -15.2}가 되겠죠.

#### 3-2. Calculate representative value by leaves

Terminal node (leaf)마다 예측결과를 평균내줍니다. 결과적으로 수많은 데이터 값이, decision tree의 최종 leaf에 따라 몇 종류의 예측값으로 축약됩니다.

![GB step 3-2 result]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-10-gradient-boosting-regression/2020-10-10-gradient-boosting-regression-step3-2-result.png)

> For \\(j=1...J_m\\) compute \\(\gamma_{jm}=\underset{\gamma}{argmin} \sum_{x_ \in R_{ij}} L(y_i, F_{m-1}(x_i) + \gamma)\\)  

이 부분 수식 푸는게 좀 복잡합니다만, 결과적으로 평균값으로 대치해주면 됩니다.

### 4. Repeat 2-3

![GB step 4: repeat 2-3]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-10-gradient-boosting-regression/2020-10-10-gradient-boosting-regression-step4.png)

다시 각 샘플에 대해 pesudo-residual을 계산하고, 이를 바탕으로 decision tree를 만드는 과정을 반복합니다.  이 때 주목할 점으로, 첫 번째 모델의 pseudo-residual보다 두 번째 모델의 pseudo-residual이 감소한 것을 확인할 수 있습니다 !

### (Test) Scale and add up the results of each tree.

![GB step 5]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-10-gradient-boosting-regression/2020-10-10-gradient-boosting-regression-test.png)

입력값 \\(x\\)에 대한 각 모델의 residual 예측값 \\(h_t(x)\\)에 동일한 Learning rate \\(\eta\\) (\\(\nu\\))를 가중치로 곱한 뒤 합계를 구합니다.

> Update \\(F_m (x)=F_{m-1} (x) + \nu \sum_{j=1}^{J_m} \gamma_{jm} I(x \in R_{jm})\\)

\\(\nu\\)는 \\(\eta\\) 대신 쓰인 learning rate 입니다.

> Output: \\(F(x)\\)

---

> 다음 포스팅에서는 Gradient Boosting for Classification 알고리즘에 대해 정리해보고자 합니다.

다음 글 보기: [부스팅 앙상블 (Boosting Ensemble) 2-2: Gradient Boosting for Classification]({{ site.url }}{{ site.baseurl }}/machine%20learning/ensemble-5-boosting-gradient-boosting-classification)
