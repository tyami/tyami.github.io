---
title: "부스팅 앙상블 (Boosting Ensemble) 1: AdaBoost"
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

이전 글 보기: [배깅 앙상블 (Bagging Ensemble): Random Forest](https://tyami.github.io/machine%20learning/ensemble-2-bagging-random-forest/)

> 이전 포스팅에서는 배깅 (Bagging) 앙상블의 대표적인 예시인 Random Forest 알고리즘에 대해 정리했습니다.  
> 이번 포스팅에서는 또 다른 앙상블 알고리즘인 부스팅 (Boosting)의 가장 첫번째 모델인 AdaBoost 알고리즘을 정리해보겠습니다. 그림은 나중에 추가하는거로 !

## AdaBoost

이전 포스팅에서 정리한 Random forest는 decision trees의 숲이라고 할 수 있습니다. 다양한 decision tree가 조화롭게 모여 일반화된 예측 결과를 만듭니다.

반면 AdaBoost (Adaptive Boosting)는 **다양한 크기**의 **stump**로 이루어진 숲으로 볼 수 있습니다.

## 3 Concepts of AdaBoost
1. Forest of stumps
2. Different weights for each stump 
3. Sequential

### 1. Forest of stumps
Random forest에서는 개별 모델로 **decision tree**를 사용합니다.

![Stumps 정의]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-08-AdaBoost/2020-10-08-stump.png)

반면, AdaBoost에서는 개별 모델로 **stump**를 사용합니다. Stump란 **한 노드와 두 개의 가지를 갖는 decision tree**로 정의할 수 있습니다. 

### 2. Different weights for each stump 
Random forest에서 모든 tree는 **동일한 weight**를 갖습니다.

![AdaBoost = Forest of stumps]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-08-AdaBoost/2020-10-08-AdaBoost.png)

반면, AdaBoost에서 각 stump는 **서로 다른 weight (amount of say)**를 갖습니다. 즉, 최종 앙상블 시, 개별 모델마다 예측 결과값에 가중치를 달리 부여합니다.

### 3. Sequential

![Random Forest = Forest of trees]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-07-AdaBoost/2020-10-07-random-forest-decision-tree.png)

Random forest에서 tree들은 **parallel**하게 만들어집니다. 즉, decision tree간 서로 영향이 없습니다.

![AdaBoost = Forest of stumps]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-08-AdaBoost/2020-10-08-AdaBoost-sequential.png)

반면, AdaBoost에서 stump들은 **sequential**하게 만들어진다는 특징을 갖습니다. 즉, 기존에 만들어진 stump가 이후 만들어질 stump 생성에 영향을 줍니다.

## AdaBoost 학습 순서
AdaBoost의 학습순서는 위와 같이 나타낼 수 있습니다. 예시를 보며 차근차근 따라해봅시다.

> 예시 그림 출처: [AdaBoost, Clearly Explained](https://www.youtube.com/watch?v=LsK-xG1cLYA) by StatQuest

### 1. Initial sample weight
![AdaBoost Step 1]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-08-AdaBoost/2020-10-08-AdaBoost-step1.png)

Initial sample weight는 모두 동일하게 해줍니다.

\\[ 
Initial\; sample\; weight=w_{i,1}=\frac{1}{total\; number\; of\; samples}
\\]

### 2. First stump

![AdaBoost Step 2]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-08-AdaBoost/2020-10-08-AdaBoost-step2.png)

Impurity (Gini index)를 바탕으로 best attribute로 분기하는 stump를 완성합니다.

### 3. Amount of say

![AdaBoost Step 3-1]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-08-AdaBoost/2020-10-08-AdaBoost-step3-1.png)

완성한 stump의 분류 결과에 따라 amount of say \\(\alpha_t\\)를 계산해줍니다. \\(\alpha_t\\)는 각 앙상블 시 각 모델의 예측값 \\(h_t\\)에 대한 가중치로 사용됩니다.

\\[
Amount\; of\; say=\alpha_t=\frac{1}{2}log(\frac{1-\epsilon_t}{\epsilon_t})
\\]

여기서 \\(\epsilon_t\\)는 Total error로 **오분류된 샘플의 sample weight 총합**으로 정의됩니다. 즉, 위 이미지에서는 한 개 샘플만 틀렸으니 1/8이 됩니다.

\\[
\epsilon_t=\sum_{
\begin{matrix}
  i=1 \\\\\\
  h_t(x_i)\neq y_i
\end{matrix}
}^n w_{i,t}
\\]

전체 Sample weights의 총합이 1이기 때문에 최고의 Stump (모두 맞춤)를 만든 경우 \\(\epsilon_t\\)는 0, 반대로 최악의 Stump (모두 틀림)를 만든 경우 \\(\epsilon_t\\)는 1을 갖습니다.

![AdaBoost Step 3-2]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-08-AdaBoost/2020-10-08-AdaBoost-step3-2.png)

또한 \\(\epsilon_t\\)에 따른 \\(\alpha_t\\)의 변화를 그림으로 그려보면, \\(\epsilon_t\\)가 낮을 때는 (<0.5)는 양의 \\(\alpha_t\\)를, \\(\epsilon_t\\)가 높을 때는 (>0.5)는 음의 \\(\alpha_t\\)를 갖습니다. 또한 \\(\lvert \alpha_t \rvert\\)는 \\(\epsilon_t\\)가 극단적인 값일 수록 (0 또는 1에 가까울수록) 더욱 더 커지는 것을 볼 수 있습니다.

위의 예시에서 계산한 amount of say \\(\alpha_t\\)는 0.97입니다.

### 4. Updated sample weight
각 sample weight \\(w_{i,t}\\)을 \\(w_{i,t+1}\\)로 업데이트할 차례입니다.  
여기서 핵심은 오분류된 샘플의 sample weight는 늘리고, 잘 분류된 샘플의 sample weight는 낮춰주는 것입니다.

#### 4.1. 오분류된 샘플
> 참고: 샘플마다 업데이트되는 가중치 양에는 차이가 없습니다.

\\[
New\; sample\; weight=w_{i,t+1}=w_{i,t} \times e^{\alpha_t}
\\]

#### 4.2. 정분류된 샘플
> 마찬가지로 샘플마다 업데이트되는 가중치 양에는 차이가 없습니다.

\\[
New\; sample\; weight=w_{i,t+1}=w_{i,t} \times e^{-\alpha_t}
\\]

#### 4.3 총합=1로 정규화
Sample weight의 총합이 1이 되도록 normalize해줍니다.

\\[
Normalized\; sample\; weight=\frac{New\; sample\; weight}{\sum New\; sample\; weight}
\\]

따라서 아래와 같이 새로운 \\(w_{i,t+1}\\)를 얻습니다.

![AdaBoost Step 4]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-08-AdaBoost/2020-10-08-AdaBoost-step4.png)

오분류된 네 번째 샘플은 가중치가 높아진 반면, 나머지 샘플들은 가중치가 낮아진 것을 확인할 수 있습니다.

### 5. Next stump
업데이트된 Sample weight를 적용한 새로운 Stump를 만듭니다. 분기 시, Impurity를 계산할 때 두 가지 방법으로 sample weight를 적용할 수 있습니다.

#### 5.1. Weighted Gini index
Sample weight를 각 샘플에 적용하여 Gini index를 계산합니다.

> 아래 수식이 맞는지는 확인이 필요합니다.

\\[
Weighted\; Gini\; index= \sum_{i=1}^C w_{i,t}p_i(1-p_i)
\\]

#### 5.2. Resampling
중복을 허용하여 Dataset을 resampling 후 Gini index를 계산합니다.

![AdaBoost Step 5-1]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-08-AdaBoost/2020-10-08-AdaBoost-step5-1.png)

- 먼저 Sample weight를 바탕으로 0~1의 구간을 나눕니다.
- 0~1값을 원래 샘플 수만큼 random sampling 하고, 이 값에 따라 원 데이터에서 데이터를 다시 sampling합니다.
- Sample weight가 큰 경우, 더 넓은 구간을 갖기 때문에, 이 샘플이 더 자주 뽑히게 됩니다.

![AdaBoost Step 5-2]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-08-AdaBoost/2020-10-08-AdaBoost-step5-2.png)

위와 같이 네번째 샘플이 많이 포함된 새로운 데이터셋이 만들어졌습니다. sample weight를 다시 \\(\frac{1}{total\; number\; of\; samples}\\)로 초기화해줍니다. (Sample weight는 일정하지만, 동일한 샘플이 더 많이 있기 때문에, 결과적으로 sample weight이 다른 것과 동일한 효과를 줍니다)

### 6. Repetition

![AdaBoost Step 6]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-08-AdaBoost/2020-10-08-AdaBoost-step6.png)

정해진 iteration만큼 stump가 생성될 때까지 3~5 과정을 반복합니다.

### 7. 최종 예측

![AdaBoost Step 7]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-08-AdaBoost/2020-10-08-AdaBoost-step7.png)

\\(N\\)번의 iteration을 진행 후, 모든 Stump 의 예측값 \\(h_t(x)\\)에 amount of say \\(\alpha_t\\)를 가중치로 곱하는 Hard voting을 통해 최종 예측값을 얻어냅니다.

\\[
F(x)=\sum_{t=1}^N \alpha_t h_t(x)
\\]

---

> 다음 포스팅에서는 또 다른 부스팅 (Boosting) 앙상블 알고리즘인 Gradient Boosting에 대해 정리해보겠습니다.

다음 글 보기: [부스팅 앙상블 (Boosting Ensemble) 2-1: Gradient Boosting for Regression](https://tyami.github.io/machine%20learning/ensemble-4-boosting-gradient-boosting-regression/)
