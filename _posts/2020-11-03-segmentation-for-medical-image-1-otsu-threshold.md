---
title: "Segmentation for MEDIA (1) Otsu threshold"
excerpt: "가장 기본적인 Segmentation 기법인 Otsu threshold를 정리해봅니다"

categories:
  - Medical image analysis

tags:
  - Medical image analysis
  - Lecture
  - Segmentation
  - Otsu threshold

toc: true
toc_sticky: true
toc_label: "Otsu threshold"

use_math: true
---

이전 포스팅: [Classification for MEDIA (7) Feature selection and extraction]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/classification-for-medical-image-7-feature-selection-extraction/)

> 이전 포스팅들에서 **Classficiation과 관련된 다양한 알고리즘들**을 정리했습니다  
> 이번 포스팅에서는 **Segmentation 기법 중 가장 기본적인 기법인 Thresholding 알고리즘**을 정리해보고자 합니다.

## Segmentation

Segmentation은 이미지 내의 각 영역을 분할하는 것을 말합니다.

![2020-11-03-segmentation-for-medical-image-01-otsu-threshold-intro.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-03-segmentation-for-medical-image-1-otsu-threshold/2020-11-03-segmentation-for-medical-image-01-otsu-threshold-intro.png)

즉, 위와 같이 뇌나 다른 부위의 MRI 같은 이미지에서, 뇌의 세부 영역들 또는 장기들의 위치를 뽑아내는 것을 말합니다. 이 때 Segmentation에 대해 관심있는 영역을 Region of Interst (ROI)라고 말합니다.

이를 통해 Longitudinal study (시간에 따른 ROI의 크기/비율의 변화)가 가능합니다. 종양을 Segmentation해서 n년 뒤 크기를 예측하는 문제를 풀어볼 수도 있겠네요.

![2020-11-03-segmentation-for-medical-image-02-contents.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-03-segmentation-for-medical-image-1-otsu-threshold/2020-11-03-segmentation-for-medical-image-02-contents.png)

Segmentation 파트에서 다룰 알고리즘들입니다. Classifciation과 마찬가지로 전통적으로 해오던 알고리즘들과 최근 Deep learning을 적용시킨 알고리즘들로 나누어 정리할 예정입니다.

전통적인 알고리즘의 경우, 사용하는 정보에 따라 Intenstiy 기반/Prior 기반/Learning 기반 등으로 점차 발전해나가는 모양을 보여줍니다.

## Thresholding

Segmentation의 가장 기본적인 알고리즘으로 Thresholding이 있습니다. 알고리즘이라고 하기도 뭐한게, 그냥 **Intensity 값이 일정 threshold보다 큰 값만 골라낸다**가 핵심입니다.

![2020-11-03-segmentation-for-medical-image-03-manual-threshold.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-03-segmentation-for-medical-image-1-otsu-threshold/2020-11-03-segmentation-for-medical-image-03-manual-threshold.png)

예를 들어, 좌측 이미지에서 Threshold=200을 기준으로 골라낼 경우 우측 이미지와 같이 나올 수 있습니다.

이 방법은 정말정말정말 간단하지만, 위 예시에서 보듯 Segmentation의 정확도가 그렇게 좋지 않습니다. 또한, Threshold 가 주관적이고 manual 하다는 단점이 존재합니다.

## Otsu thresholding

![2020-11-03-segmentation-for-medical-image-04-otsu-threshold.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-03-segmentation-for-medical-image-1-otsu-threshold/2020-11-03-segmentation-for-medical-image-04-otsu-threshold.png)

Otsu thresohlding은 1979년, Nobuyuki Otsu라는 일본사람이 제안한 Automatic thresholding 방법입니다. Otsu thresholding에서는 위와 같이 데이터를 Intenstity 값에 따라 히스토그램화시킵니다.

![2020-11-03-segmentation-for-medical-image-05-step1.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-03-segmentation-for-medical-image-1-otsu-threshold/2020-11-03-segmentation-for-medical-image-05-step1.png)

그리고 관심있는 영역인 Foreground와 Background의 Intensity가 다르다고 가정합니다. 따라서 Foreground와 Background는 다른 분포를 띌 것이라고 생각할 수 있습니다. 이제 일정 Threshold 에 따라 Background/Foreground로 히스토그램을 분리합니다.

Threshold의 선택 방법은 후술합니다.

분리된 두 개의 히스토그램에 대해서 데이터의 비율 \\(w\\), 데이터의 평균 intensity \\(\mu\\), 그리고 데이터의 분산 \\(\sigma\\)를 계산합니다

![2020-11-03-segmentation-for-medical-image-06-step2.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-03-segmentation-for-medical-image-1-otsu-threshold/2020-11-03-segmentation-for-medical-image-06-step2.png)

먼저 Background에 대해서 계산해보면 위와 같습니다.

![2020-11-03-segmentation-for-medical-image-07-step3.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-03-segmentation-for-medical-image-1-otsu-threshold/2020-11-03-segmentation-for-medical-image-07-step3.png)

Foreground에 대해서도 똑같이 계산합니다.

Otsu thresholding에서는 두 히스토그램을 최대한 분리시키는 것을 목표로 합니다. 따라서 히스토그램간 분산 \\(\sigma_B\\)는 최대화하되, 히스토그램 내 분산 \\(\sigma_W\\)은 최소화해야합니다. 이를 수식으로 표현하면 아래와 같습니다.

\\[
maximize \frac{\sigma_B^2(T)}{\sigma_W^2(T)}
\\]

![2020-11-03-segmentation-for-medical-image-08-step4.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-03-segmentation-for-medical-image-1-otsu-threshold/2020-11-03-segmentation-for-medical-image-08-step4.png)

위 식에 따라 \\(\frac{\sigma_B^2(T)}{\sigma_W^2(T)}\\)를 계산합니다. 위 예시에서는 0.512가 나왔습니다.

![2020-11-03-segmentation-for-medical-image-09-step5.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-03-segmentation-for-medical-image-1-otsu-threshold/2020-11-03-segmentation-for-medical-image-09-step5.png)

히스토그램의 모든 분계점마다 \\(\frac{\sigma_B^2(T)}{\sigma_W^2(T)}\\)를 계산해서 최대값을 갖는 지점을 Threshold 로 설정합니다.

### Drawback of Otsu thresholding

![2020-11-03-segmentation-for-medical-image-10-drawback.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-03-segmentation-for-medical-image-1-otsu-threshold/2020-11-03-segmentation-for-medical-image-10-drawback.png)

Threshold의 두 문제점 중 manual하다는 문제점은 해결했습니다. 하지만, Otsu threshold 역시 데이터를 단지 두 그룹으로만 분리하기 때문에, Otsu threshold만으로는 깔끔한 Segmentation이 불가능합니다. 위와 같이 여러 종류의 Noise들이 포함되어 있는 것을 확인할 수 있습니다.

---

> 다음 포스팅에서는 Thresholding 후 Segmentation 결과를 개선하기 위한 방법으로 **Morphological processing**을 정리해보고자 합니다

다음 포스팅: [Segmentation for MEDIA (2) Morphological processing]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/segmentation-for-medical-image-2-morphological-processing/)
