---
title: "Classification for MEDIA (3) Convolutional neural network"
excerpt: "Convolutional neural network (CNN)에 대해 정리해봅니다"

categories:
- Medical image analysis

tags:
- Medical image analysis
- Lecture
- Classification
- Deep learning
- Convolutional neural network

toc: true
toc_sticky: true
toc_label: "Classification for MEDIA (3) CNN"

use_math: true
---

이전 포스팅: [Classification for MEDIA (2) Deep neural network]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/MEDIA-4-classification-for-medical-image-2-DNN/)

> 이전 포스팅에서는 Medical Image Analysis 에 사용되는 Deep neural network (DNN)에 대해 간단하게 정리해보았습니다.  
> 이번 포스팅에서는 이미지 데이터에 많이 활용되는 Convolutional neural network (CNN)의 기초에 대해 정리해보고자 합니다.

## Convolutional neural network

Convolutional neural network는 Convolution 기법을 활용한, 이미지에서 많이 사용되는 뉴럴 네트워크 구조입니다.

일반적인 DNN 모델에 비해 CNN 모델은 파라미터 수를 줄이면서도 이미지의 local property를 학습시킬 수 있다는 장점을 지닙니다.

### Convolution

Convolution을 먼저 정리해봅시다.

![2020-10-25-medical-image-classification-3-cnn-1-data-filter]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-1-data-filter.png)

Convolution은 주어진 인풋을 Convolution filter가 훑고 지나가면서 1:1 연산을 수행합니다. 이 때 Filter의 면적(?)를 receptive field라고도 합니다.

예시를 통해 작동방식을 살펴봅시다.

예시에서는 Convolutional filter의 모든 weight 가 동일한 값 \\(\frac{1}{9}\\)로 연산됩니다.

![2020-10-25-medical-image-classification-3-cnn-2-convolution-1]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-2-convolution-1.png)

맨 처음 좌상단에서는 입력 이미지의 모든 값에 \\(\frac{1}{9}\\)이 곱해지면서, 최종 출력값이 9가 됩니다.

![2020-10-25-medical-image-classification-3-cnn-2-convolution-2]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-2-convolution-2.png)

옆과 아래로 한 칸씩 이동하면서 연산을 계속합니다.

![2020-10-25-medical-image-classification-3-cnn-2-convolution-3]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-2-convolution-3.png)

계속해서 이동하면서 연산합니다.

![2020-10-25-medical-image-classification-3-cnn-2-convolution-4]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-2-convolution-4.png)

Convolution filter로 입력값의 모든 부분을 훑어낸다면 최종 출력을 얻을 수 있습니다.

![2020-10-25-medical-image-classification-3-cnn-3-different-filter]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-3-different-filter.png)

Convolution filter는 각 weight를 다르게 지정할 수도 있습니다.

![2020-10-25-medical-image-classification-3-cnn-4-diverse-filters]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-4-diverse-filters.png)

또한 width와 height의 크기도 자유롭게 바꿀 수 있습니다. 다양한 종류의 filter가 나올 수 있으며, CNN에서는 바로 이 convolutional filter를 학습합니다.

#### Role of convolution

![2020-10-25-medical-image-classification-3-cnn-5-filter-role]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-5-filter-role.png)

Convolution의 역할이 와닿지 않을까봐 준비했습니다. Filter에 따라 다양한 역할을 수행합니다. Edge를 detection 하기도 하고, 이미지를 뿌옇게 (Blurred) 하거나 선명하게 (Sharpen) 하기도 합니다.

---

### Convolutional layer

![2020-10-25-medical-image-classification-3-cnn-6-pooling-layer]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-6-pooling-layer.png)

Convolutional layer는 앞서 정리한 Convolution을 활용한 layer입니다. 실험자가 원하는 크기, 개수의 convolution filter를 지정하여 각 filter의 weight를 학습시킵니다.

#### RGB image convolution

![2020-10-25-medical-image-classification-3-cnn-7-multi-channel-input]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-7-multi-channel-input.png)

만약 입력이 RGB 이미지라면 입력 채널이 3개가 됩니다. 이 경우, Convolution filter의 depth도 3채널로 구성합니다. 즉, 파라미터 수도 3배가 됩니다.

Depth는 변하지만, 각 셀의 연산값을 다 합친 하나의 값만 남게되기 때문에 출력의 depth는 변함이 없습니다.

일반적으로 DNN에서 레이어가 거듭될수록 노드 수를 늘리는 것처럼, CNN에서는 Filter의 개수를 늘립니다. 즉, 뒤로 갈 수록 filter의 depth가 점점 두꺼운 형태를 띄게 됩니다.

#### Stride

![2020-10-25-medical-image-classification-3-cnn-8-stride]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-8-stride.png)

위의 Convolution 예시에서는 Filter가 한 칸씩 이동했지만, 실제 모델을 구현할 때는 이미지 크기를 강제로 줄이기 위한 목적으로 Filter를 여러 칸씩 이동하기도 합니다. 이 때 이동거리를 **stride**라고 부릅니다.

Stride가 커질 수록, 이미지를 더 듬성듬성 훑게되니, 출력은 작아지게 됩니다.

#### Padding

![2020-10-25-medical-image-classification-3-cnn-9-padding]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-9-padding.png)

Padding은 출력 이미지의 크기를 조절하기 위한 방법으로 사용됩니다.

입력값의 외곽에 빈 값을 넣어 입력 이미지를 강제로 크게 만듭니다. 입력 이미지가 커지니 출력 이미지도 당연히 커지겠지요.

---

### Pooling layer

Pooling layer는 파라미터를 증가시키지 않으면서 이미지 크기를 줄이고 싶을 때 사용합니다. Max pooling과 Average pooling이 대표적입니다.

![2020-10-25-medical-image-classification-3-cnn-10-maxpooling]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-10-maxpooling.png)

Max pooling은 filter의 receptive field 내의 값 중 가장 큰 값만 남깁니다.

![2020-10-25-medical-image-classification-3-cnn-10-avgpooling]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-10-avgpooling.png)

Average pooling은 filter의 receptive field 내의 값들의 평균값을 남깁니다.

---

### Fully-Connected layter

![2020-10-25-medical-image-classification-3-cnn-11-fully-connected-layers]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-11-fully-connected-layers.png)

Convolutional layer와 Pooling layer를 쌓아 이미지 크기를 어느 정도 감소시키고 나면, 어느 순간에는 DNN에서 사용되는 Fully-connected layer를 연결할 일이 생깁니다. (최근에는 Fully Convolutional layer라고 해서 Fully-Connected layer를 쓰지 않는 구조도 있습니다)

이 때 Flatten layer를 이용하여, Width X Height X Depth 를 한 줄의 벡터로 쭉 펴줍니다.

---

### Overall pipeline

![2020-10-25-medical-image-classification-3-cnn-12-simple-cnn]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-12-simple-cnn.png)

CNN의 기본적인 파이프라인은 위와 같습니다.

- Convolutional layer와 Pooling layer를 반복적으로 쓰면서 이미지 크기를 줄이고 Depth를 키워나갑니다.
- 어느 정도 사이즈가 줄었다면, Flatten layer로 쭉 펴줍니다.
- Fully-connected layer를 사용해서 최종 분류를 수행합니다.

## 3D CNN

의료 영상 데이터는 3D 이미지인 경우가 많다고 정리했습니다. 3D 이미지에 대한 CNN도 별반 다르지 않습니다.

![2020-10-25-medical-image-classification-3-cnn-13-3d-cnn]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-13-3d-cnn.png)

2D 이미지에서 dimension이 하나 더 늘어났습니다. 이에 맞추어 Filter의 dimension도 하나 더 늘려줍니다. 결과도 마찬가지로 dimension이 하나 더 늘어납니다.

![2020-10-25-medical-image-classification-3-cnn-14-3d-cnn-multi-channel]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-14-3d-cnn-multi-channel.png)

만약 T1-weighted, T2-weighted 등 여러 종류의 3D 이미지를 함께 입력으로 사용하는 경우엔, 마지막 dimension이 커집니다. 흑백 이미지에서 RGB 이미지로 갈 때의 변화를 생각하면 쉽습니다.

![2020-10-25-medical-image-classification-3-cnn-14-3d-cnn-with-demographic-score]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-14-3d-cnn-with-demographic-score.png)

의료영상 데이터는 Demographic score를 활용하는 경우가 많다고 했었습니다. CNN 모델에 대입해도 마찬가지로 이런 형태가 됩니다. CNN 을 통해 학습시킨 feature를 demographic score로 normalize로 학습시켜 사용할 수 있습니다.

---

Convolutional Neural Network의 기초적인 구성에 대해 정리해보았습니다. CNN은 파라미터 수를 줄이면서도 좋은 성능을 내는 만큼, 정말 다양한 구조가 존재합니다.

> 다음 포스팅에서는 주요 CNN 모델들에 대해 정리해보도록 하겠습니다.

다음 포스팅: [Classification for MEDIA (4) Advanced CNN models]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/MEDIA-6-classification-for-medical-image-4-advanced-CNN/)
