---
title: "Segmentation for MEDIA (6) Atlas-based methods and label fusion"
excerpt: " Atlas-based segmentation methods와 label fusion 기법을 정리해봅니다"

categories:
  - Medical image analysis

tags:
  - Medical image analysis
  - Lecture
  - Atlas-based segmentation
  - Label fusion
  - Voting
  - Registration

toc: true
toc_sticky: true
toc_label: "Atlas-based methods and label fusion"

use_math: true
---

이전 포스팅: [Segmentation for MEDIA (5) Active contour model]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/segmentation-for-medical-image-5-active-contour-model/)

> 이전 포스팅에서는 Shape에 대한 Prior 정보를 바탕으로 최적의 contour를 찾아내는 **Active contour model**을 정리했습니다  
> 이번 포스팅에서는 **Atlas-based segmentation methods와 label fusion 기법**을 정리해보고자 합니다.

## Atlas-based segmentation method

### Atlas

![2020-11-09-segmentation-for-medical-image-6-atlas-based-methods-01.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-09-segmentation-for-medical-image-6-atlas-based-methods/2020-11-09-segmentation-for-medical-image-6-atlas-based-methods-01.png)

일반적으로 Classification에서 Training data라고 불리우는 영상 \\(y_i \\)와 Label \\(z_i\\)의 조합을 Atlas라고 말합니다. 즉, Atlas 정보를 바탕으로 Test data \\(x\\)에 대해 segmentation을 수행합니다.

### Registration

Atlas를 기반으로 하는 알고리즘의 경우 Registration을 이용합니다. Registration은 주어진 틀에 영상을 맞추는 과정입니다.

![2020-11-09-segmentation-for-medical-image-6-atlas-based-methods-02.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-09-segmentation-for-medical-image-6-atlas-based-methods/2020-11-09-segmentation-for-medical-image-6-atlas-based-methods-02.png)

1번째 Atlas 중 입력 영상 \\(y_1\\)를 Test 이미지 \\(x\\)에 registration시키는 함수 \\(T\\)를 학습(?)합니다. 이 함수 \\(T\\)는 \\(y_i\\)의 영상 이미지를 최대한 \\(x\\)에 가깝게 변환하는 함수입니다.

이제 label 이미지 \\(z_i\\)를 \\(T\\) 변환시킵니다. 그러면 새로운 label 이미지 \\(T(z_i)\\)를 얻을 수 있습니다.

![2020-11-09-segmentation-for-medical-image-6-atlas-based-methods-03.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-09-segmentation-for-medical-image-6-atlas-based-methods/2020-11-09-segmentation-for-medical-image-6-atlas-based-methods-03.png)

2번째 Atlas에 대해서도 위와 같은 과정을 반복합니다. 이 과정을 N개의 Atlas 이미지에 대해 수행합니다.

### Voting

![2020-11-09-segmentation-for-medical-image-6-atlas-based-methods-04.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-09-segmentation-for-medical-image-6-atlas-based-methods/2020-11-09-segmentation-for-medical-image-6-atlas-based-methods-04.png)

그러면 N개의 segmentation result를 얻습니다. 이 결과를 Voting해서 최종 Segmentation 결과를 얻습니다.

- Voting 개념 참고 포스팅: [앙상블 (Ensemble)의 기본 개념]({{ site.url }}{{ site.baseurl }}/machine%20learning/ensemble-1-basics/#voting의-종류)

### Weighted voting

![2020-11-09-segmentation-for-medical-image-6-atlas-based-methods-05.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-09-segmentation-for-medical-image-6-atlas-based-methods/2020-11-09-segmentation-for-medical-image-6-atlas-based-methods-05.png)

이 때, \\(y_i\\)와 \\(x\\) 간 유사도를 계산하여 이를 voting 시 weight로 적용할 수도 있습니다.

## Drawbacks of atlas-basd method

Atlas 기반의 알고리즘은 atlas가 많아질 수록 더 좋은 성능을 내지만, 그에 따라 연산량이 증가한다는 단점을 갖습니다.

또한 Registration (T 변환)이 얼마나 잘 되느냐에 따라 Segmentation의 영향을 받습니다.

Non-rigid (?) registration을 사용할 경우 높은 정확도를 보이지만 위에 언급한 바와 같이 연산량이 증가합니다 (=느립니다).

빠른 속도를 위해서는 Registration 시 Rotation, Translation뿐 아니라 Shearing과 Scaling을 사용한 Affine registration을 활용할 수 있지만, 이 경우 정확도가 떨어져서 \\(T(z_i)\\)를 바로 사용하는게 아니라, 별도의 후처리가를 수행합니다. 이 후처리 과정을 label fusion 이라고 합니다.

## Label fusion

![2020-11-09-segmentation-for-medical-image-6-atlas-based-methods-06.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-09-segmentation-for-medical-image-6-atlas-based-methods/2020-11-09-segmentation-for-medical-image-6-atlas-based-methods-06.png)

Label fusion은 Patch를 이용합니다.

입력 영상 \\(y_i\\)의 특정 영역에 대해 작은 Patch 이미지를 추출합니다. 동일한 크기의 Patch를 Test 영상 \\(x\\)의 모든 부분을 훑게 하면서, 두 패치간 유사도를 계산합니다. 계산된 유사도를 weight 값으로 사용하여, \\(T(z_i)\\)에 픽셀별로 곱해줍니다. 그러면 위 그림처럼 확률값을 갖는 이미지를 얻을 수 있게 되고, 이를 바탕으로 segmentation을 진행하는 것을 label fusion이라고 합니다.

### Advantage of label fusion

Label fusion 방법은 사람마다 shape이 유사한 장기를 segmentation할 때, 별도의 학습 없이도 높은 성능을 발휘한다고 알려져 있습니다.

또한 연산량을 줄이기 위해, 모든 Atlas에 대해 연산하는 것이 아니라, 유사도가 높은 Atlas 들만 골라 사용하는 방법도 있습니다.

---

> 다음 포스팅에서는 **학습 기반의 Segmentation 알고리즘**을 정리해보고자 합니다

다음 포스팅: [ 작성중 ]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/ 작성중 /)
