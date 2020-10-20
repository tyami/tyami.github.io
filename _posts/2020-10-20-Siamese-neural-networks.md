---
title: "Siamese Neural Networks (샴 네트워크) 개념 이해하기"
excerpt: "Siamese Neural Networks (샴 네트워크)를 이해해봅시다."

categories:
- Deep learning

tags:
- Deep learning
- Siamese neural network

toc: true
toc_sticky: true
toc_label: "Siamese Neural Networks"

use_math: true
---

> 이번 포스팅에서는 Siamese neural network에 대해 정리해보고자 합니다.  
> 최근 SMC 2020 학회에서 발표된 연구 중 Siamese neural network를 사용한 연구가 있어서, 관련 내용을 공부하는 김에 정리하게 되었습니다. 

## Siamese Neural Networks

![쌍둥이]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-20-siamese-neural-networks/2020-10-20-siamese-neural-networks-1-twins.jpg)

쌍둥이 (Twins)는 거의 동일한 생김새를 지녔습니다 (이란성 쌍둥이는 넘어가도록 합니다...)

![쌍둥이]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-20-siamese-neural-networks/2020-10-20-siamese-neural-networks-2-siamese-twins.jpg)

이 중, **샴 쌍둥이**라는 쌍둥이들은 비슷한 생김새뿐 아니라, 조금 더 특별합니다. 그들은 신체의 일부를 공유합니다.

![샴 네트워크]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-20-siamese-neural-networks/2020-10-20-siamese-neural-networks-3-siamese-neural-networks.png)

Siamese Neural Networks (샴 네트워크)는 샴 쌍둥이에서 착안된 네트워크입니다. **두 네트워크의 구조가 서로 닮아있으며, 더 나아가 weight를 공유합니다**

### Paper (2015)

![샴 네트워크 paper]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-20-siamese-neural-networks/2020-10-20-siamese-neural-networks-4-paper.png)

샴 뉴럴 네트워크의 **샴 네트워크** 구조 자체는 2005년 [Learning a Similarity Metric Discriminatively, with Application to Face
Verification](http://yann.lecun.com/exdb/publis/pdf/chopra-05.pdf)라는 논문으로 Yann LeCun 교수 연구팀에 의해 발표되었습니다. 그러다 2015년, [Siamese Neural Networks for One-shot Image Recognition](https://www.cs.cmu.edu/~rsalakhu/papers/oneshot1.pdf)라는 이름으로 Neural network를 접목시킨 **샴 뉴럴 네트워크**가 발표되었습니다. 논문 제목에서 느껴지듯 One-shot learning 분야의 논문입니다.

하지만 오늘 포스팅에서는 Siamese neural network의 이해에 집중하기 위해, 논문 내용과 One-shot learning에 대해서는 나중에 별로 포스팅으로 따로 정리하겠습니다. 

#### One-shot learning

그래도 **One-shot learning**이 뭔지는 짚어둬야 설명이 가능합니다.

딥러닝은 매우 강력한 분류 모델입니다. 하지만 어마무시한 양의 파라미터를 가진 딥러닝 모델을 훈련시키기 위해서는 어마어마어마무시한 양의 데이터가 필요합니다. 모든 데이터에 대해 라벨링을 해서 이를 토대로 학습합니다. 어떻게 보면 가장 무식한 방법으로 학습을 진행하고 있는 것이죠.

반면, 사람은 적은 데이터로도 학습이 가능합니다.

![직관]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-20-siamese-neural-networks/2020-10-20-siamese-neural-networks-5-intuition-training.png)

위와 같이 멋진 라이플 총기들을 "총"으로 라벨링하여 딥러닝 모델을 학습시켰다고 합시다.

![직관]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-20-siamese-neural-networks/2020-10-20-siamese-neural-networks-6-intuition-test1.png)

이 모델은 총신, 탄창, 손잡이, 개머리판 등 몇몇 총에 대한 특징을 찾아내어, 다양한 라이플 총기를 기가 막히게 총으로 인식합니다.

![직관]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-20-siamese-neural-networks/2020-10-20-siamese-neural-networks-7-intuition-test2.png)

그런데 이런 총은 어떨까요? 권총이 개발되기도 전의 옛날 총기들 보면, 사람은 몇 장의 사진을 보고도 '아 총이구나'라고 하겠지만, 우리의 어마무시한 딥러닝 모델은 이게 총이라고 인식하기 힘들어 합니다. 학습 데이터에서 보지 못했던 형태의 총이기 때문이죠.

딥러닝 모델이 인간처럼 소량의 데이터만으로 학습을 할 수 있게 하는 것을 **Few-shot learning**이라고 합니다. **One-shot learning**은 few-shot learning의 극단적인 예시로, 한 장의 데이터만으로 학습을 할 수 있게 만드는 것을 말합니다.

### Architecture

![siamese neural network]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-20-siamese-neural-networks/2020-10-20-siamese-neural-networks-8-siamese-neural-network.png)

앞서 잠깐 본 것 처럼, 샴네트워크는 weight를 공유하는 두 네트워크로 이루어집니다. 

![siamese neural network]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-20-siamese-neural-networks/2020-10-20-siamese-neural-networks-9-siamese-neural-network-another-figure.png)

어차피 weight를 공유하기 때문에, 한 네트워크라고 봐도 무방합니다.

#### Training

1. 두 개의 입력 데이터 (Input 1, Input 2)를 준비합니다.
2. 각 입력에 대한 임베딩 값 (Embedding 1, Embedding 2)을 얻습니다.
3. 두 임베딩 사이의 거리를 계산합니다. L1 norm, L2 norm 등의 방법을 사용합니다.
4. 두 입력이 같은 클래스에 속한다면 거리를 가깝게, 다른 클래스에 속한다면 거리를 멀게 Siamese neural network를 학습시킵니다.

![siamese neural network 학습 결과]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-20-siamese-neural-networks/2020-10-20-siamese-neural-networks-10-feature-extractor.png)

학습이 완료된 후, 모든 학습 데이터에 대한 임베딩 값을 뿌렸을 때, 같은 클래스끼리는 모이고 다른 클래스끼리는 멀어지는 결과를 보여줍니다.

### Advantage

#### 1. Low sample

샴 네트워크는 One-shot learning을 위해 개발되었습니다. 즉, 소량의 데이터만으로 학습이 가능하다는 장점을 지닙니다.

#### 2. Feature extraction network

![siamese neural network 학습 결과]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-20-siamese-neural-networks/2020-10-20-siamese-neural-networks-11-feature-extractor.png)

Siamese neural network는 결과적으로 입력 데이터를 임베딩으로 변환시킵니다. 즉, 변환되는 임베딩의 차원 수로 feature extraction (또는 reduction) 하는 네트워크로 볼 수 있습니다. 임베딩으로 변환한 후, 뒤에 별도의 k-NN, MLP 등의 classifier를 붙여 feature extractor로 사용 가능합니다.

---

## Utilization example

제가 사용하는 뇌 데이터는 인간 실험을 통해 데이터가 수집되고, 노이즈에 취약한 특징 때문에, 샘플 수가 매우 한정적인 데이터입니다. Siamese neural network의 활용을 짧게 생각해본 결과, 뇌 데이터에는 Siamese neural network의 두 가지 장점이 모두 활용될 수 있을 것 같습니다.

1. (Biometric 주제에 대해) few-shot learning 패러다임을 뇌 데이터 기반 biometrics 연구에 활용할 수 있을 것 같습니다.
2. (보다 다양한 주제에 대해) 뇌 데이터에서 유의미한 feature를 추출하는 하나의 방법으로 siamese neural network가 사용될 수 있을 것으로 생각됩니다.