---
title: "앙상블 (Ensemble)의 기본 개념"
excerpt: "앙상블 모델의 개념과 종류를 정리해봅시다"

categories:
- Machine learning

tags:
- Machine learning
- Ensemble
- Algorithm
- Voting
- Bagging
- Boosting
- Stacking

toc: true
toc_sticky: true
toc_label: "앙상블의 기본 개념"

use_math: true
---

이전 글 보기: [의사결정 나무 (Decision Tree) CART 알고리즘 설명](https://tyami.github.io/machine%20learning/decision-tree-4-CART/)

> 이전 포스팅들에서는 의사결정 나무의 여러 알고리즘과 단점에 대해 정리해보았습니다. 바로 **과적합에 취약**하다는 점입니다.  
> 이번 포스팅에서는 과적합 문제를 해결하기 위한 방법으로 앙상블 (Ensemble) 알고리즘을 설명하고, 그 종류를 알아보도록 하겠습니다.

## 앙상블 (Ensemble)

![Musical Ensemble](https://theclassicalnovice.files.wordpress.com/2015/05/dublin_philharmonic_orchestra_performing_tchaikovskys_symphony_no_4_in_charlotte_north_carolina-e1431911721232.jpg?w=1180&h=435&crop=1)

**앙상블 (Ensemble)**을 **통일, 조화**를 뜻하는 프랑스어입니다. 주로 음악에서 여러 악기에 협주를 뜻하는 말로 사용됩니다. 많은 수의 작은 악기소리가 조화를 이루어 더욱 더 웅장하고 아름다운 소리를 만들어냅니다. 물론 그래서는 안 되겠지만, 한 명의 아주 작은 실수는 다른 소리에 묻히기도 합니다.

기계학습에서의 앙상블도 이와 비슷합니다. 여러 개의 weak learner들이 모여 투표 (voting)를 통해 더욱 더 강력한 strong learner를 구성합니다. 많은 모델이 있기 때문에, 한 모델에서 예측을 엇나가게 하더라도, 어느 정도 보정이 됩니다. 즉, 보다 일반화된 (generalized) 모델이 완성되는 것입니다.

단일 모델로는 Decision tree, SVM, Deep learning 등 모든 종류의 학습 모델이 사용될 수 있습니다. 

---

### Voting의 종류

최종 모델의 예측값을 결정짓는 Voting은 크게 **하드 보팅 (Hard voting)**과 **소프트 보팅 (Soft voting)**으로 나눌 수 있습니다.

#### 하드 보팅 (Hard voting)

하드 보팅은 **각 weak learner들의 예측 결과값을 바탕으로 다수결 투표**하는 방식입니다. 

![Hard voting]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-06-ensemble/2020-10-06-ensemble-hard-voting.png)

빨간공인지 파란공인지 예측하는 Binary classification 문제에서 동일한 데이터에 대해 각 분류기는 클래스별 예측 확률을 제시합니다. 이 확률값에 따라 최종 예측값이 계산되는데, 하드 보팅은 이 예측값의 다수결 투표로 최종 예측값을 결정합니다.

따라서 다섯 개 분류기 중 빨간 공으로 예측한 분류기가 3개이니, 이 샘플에 대한 최종 예측값은 빨간 공이 됩니다.

#### 소프트 보팅 (Soft voting)

반면 소프트 보팅은 **weak learner들의 예측 확률값의 평균 또는 가중치 합**을 사용합니다.

- **평균 (average)**

![Soft voting average]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-06-ensemble/2020-10-06-ensemble-soft-voting-average.png)

weak learner 개별의 예측값은 중요하지 않습니다. 예측 확률값을 단순 평균내어 확률이 더 높은 클래스를 최종 예측값으로 결정합니다.

동일한 예시에서 빨간 공으로 예측한 세 개 분류기 (1, 2, 4)의 클래스별 예측 확률은 크게 차이나지 않습니다. 반면, 파란 공으로 예측한 두 개 분류기 (3, 5)는 높은 확률로 파란 공으로 예측했습니다. 그 결과, 최종 예측값은 파란 공이 됩니다.

- **가중치 합 (weighted sum)**

![Soft voting weighted sum]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-06-ensemble/2020-10-06-ensemble-soft-voting-weighted-sum.png)

만약 어떠한 이유 (단일 모델에 사용되는 feature engineering 방법 등) 로 weak learner들에 대한 신뢰도가 다를 경우, 가중치를 부여하여 확률값의 평균이 아닌 가중치 합을 사용할 수도 있습니다.

동일한 예시에서 각 분류기의 예측확률값에 상이한 가중치를 부여하였기에, 최종 예측값이 빨간 공이 되는 것을 확인할 수 있습니다.

가중치는 임의로 부여할 수도 있고, 뒤에서 다시 언급할 스태킹 (Stacking) 기법을 사용할 수도 있습니다.

---

### 앙상블의 종류

![Bagging과 Boosting]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-06-ensemble/2020-10-06-ensemble-bagging-boosting.png)  
[출처](https://quantdare.com/what-is-the-difference-between-bagging-and-boosting/)

앙상블 알고리즘은 학습 방식에 따라 크게 **배깅 (Bagging)**, **부스팅 (Boosting)**, 그리고 **스태킹 (Stacking)**으로 나눌 수 있습니다. 본 포스팅에서는 개념만 간단히 정리하고, 추후 포스팅을 통해 예시를 추가로 정리하도록 하겠습니다.

#### 배깅 (Bagging)

배깅 (Bagging)은 **B**ootstrap **Agg**regat**ing**의 약자입니다. 이름에서 알 수 있다시피 **부트스트랩 (Boostrap)**을 이용합니다.

![Bootstrap]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-06-ensemble/2020-10-06-bootstrap.png) 

부트스트랩이란 주어진 데이터셋에서 random sampling 하여 새로운 데이터셋을 만들어내는 것을 의미합니다. 부트스트랩을 통해 만들어진 여러 데이터셋을 바탕으로 weak learner를 훈련시킨 뒤, 결과를 voting 합니다.

대표적인 예시로 Random Forest가 있습니다. 

- [배깅 앙상블 (Bagging Ensemble): Random Forest](https://tyami.github.io/machine%20learning/ensemble-2-bagging-random-forest/)

#### 부스팅 (Boosting)

부스팅 (Boosting)은 반복적으로 모델을 업데이트해나갑니다. 이 때 이전 iteration의 결과에 따라 데이터셋 샘플에 대한 가중치를 부여합니다. 결과적으로, 반복할 때마다 각 샘플의 중요도에 따라 다른 분류기가 만들어지게 됩니다. 최종적으로는 모든 iteration에서 생성된 모델의 결과를 voting합니다.

아래는 Boosting 과정을 대략적으로 나타낸 그림입니다.

![Boosting 과정]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-06-ensemble/2020-10-06-ensemble-boosting.png)  
[출처](https://pubmed.ncbi.nlm.nih.gov/30006563/)

Iteration 1에서 빨간 원은 잘못 분류된 샘플입니다. 이 샘플에 대한 가중치를 높여 분류기를 다시 만듭니다.

Iteration 2에서는 Iteration 1의 분류기과 새로운 분류기를 함께 사용하여 분류합니다. 그 결과 파란 사각형이 잘못 분류되었습니다. 이 샘플에 대한 가중치를 높여 분류기를 다시 만듭니다.

동일한 방식으로 반복을 진행한 뒤, 만들어진 분류기들을 모두 결합하여 최종 모델을 만들어냅니다.

Boosting은 다시 **Adaptive Boosting (AdaBoost)**와 **Gradient Boosting Model (GBM)** 계열로 나눌 수 있습니다.

- [부스팅 앙상블 (Boosting Ensemble) 1: AdaBoost](https://tyami.github.io/machine%20learning/ensemble-3-boosting-AdaBoost/)
- 부스팅 앙상블 (Boosting Ensemble) 2: Gradient Boosting

정확도와 속도를 개선한 최근 부스팅 알고리즘들은 Kaggle 등 데이터분석 대회에서 많이 사용되고 있습니다.

- 부스팅 앙상블 (Boosting Ensemble) 3: XGBoost
- 부스팅 앙상블 (Boosting Ensemble) 4: LightGBM
- 부스팅 앙상블 (Boosting Ensemble) 5: CatBoost
- 부스팅 앙상블 (Boosting Ensemble) 6: NGBoost

#### 스태킹 (Stacking)
> 개인적으로 스태킹은 이 곳보다는 보팅의 종류에 언급하는 게 더 어울리는 것 같긴 하지만, 대부분 배깅, 부스팅과 함께 비교하기에 넣었습니다.

스태킹은 weak learner들의 예측 결과를 바탕으로 meta learner로 학습시켜 최종 예측값을 결정하는 것을 말합니다. meta learner 또한 학습이 필요하며, 이 때 사용되는 데이터는 training data에 대한 각 weak learner들의 예측 확률값의 모음입니다. 과적합 방지를 위해 주로 k-fold cross validation을 이용합니다.

그림으로 설명하기가 좀 애매해서 추후 포스팅에서는 코드로 설명해보도록 하겠습니다. 그나마 아래 그림이 스태킹을 잘 나타내주는 것 같습니다.

![스태킹 설명](https://lh3.ggpht.com/-oev_BuVObEs/UL7oEHlO53I/AAAAAAAADEs/i6Lv1-4GRDE/s1600/StackingCropped.png)

---

> 다음 포스팅에서는 배깅 (Bagging)의 대표적인 알고리즘인 Random Forest에 대해 정리해보도록 하겠습니다.

다음 글 보기: [배깅 앙상블(Ensemble): Random Forest](https://tyami.github.io/machine%20learning/ensemble-2-bagging-random-forest/)