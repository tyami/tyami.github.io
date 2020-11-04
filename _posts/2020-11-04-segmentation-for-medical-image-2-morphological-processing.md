---
title: "Segmentation for MEDIA (2) Morphological processing"
excerpt: "Thresholding의 후처리 방법인 Morphological processing를 정리해봅니다"

categories:
- Medical image analysis

tags:
- Medical image analysis
- Lecture
- Segmentation
- Morphological processing
- Dilation
- Erosion
- Opening
- Closing

toc: true
toc_sticky: true
toc_label: "Morphological processing"

use_math: true
---

이전 포스팅: [Segmentation for MEDIA (1) Otsu threshold]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/segmentation-for-medical-image-1-otsu-threshold/)

> 이전 포스팅에서는 **Segmentation 기법 중 가장 기본적인 기법인 Thresholding 알고리즘**을 정리했습니다  
> 이번 포스팅에서는 Threshold 이후 후처리에 사용되는 **Morphological processing**을 정리해보고자 합니다.


## Drawback of Otsu thresholding

![2020-11-03-segmentation-for-medical-image-10-drawback.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-03-segmentation-for-medical-image-1-otsu-threshold/2020-11-03-segmentation-for-medical-image-10-drawback.png)

이전 포스팅에서 정리한 것처럼 Otsu threshold를 포함한 Thresholding 방법은 데이터를 두 개의 그룹으로 분리하기 때문에, 깔끔한 Segmentation이 불가능합니다. 따라서 결과 이미지를 보면 위와 같이 여러 종류의 Noise들이 포함되어 있는 것을 확인할 수 있습니다.

## Morphological processing

Morphological processing은 주변 데이터와의 관계를 이용해 이러한 노이즈를 제거할 수 있는 방법입니다.

Dilation과 Erosion이라는 개념이 등장합니다. 이는 [이전의 포스팅]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/2020-10-25-MEDIA-5-classification-for-medical-image-3-CNN/)에서 정리했던 Convolution과 비슷합니다.

### Convolution

![2020-10-25-medical-image-classification-3-cnn-2-convolution-2]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-25-medical-image-classification-3-CNN/2020-10-25-medical-image-classification-3-cnn-2-convolution-2.png)

이전 포스팅에서 정리한 Convolution에서는 필터가 입력 이미지를 훑고 지나가면서, 입력 이미지를 압축하는 효과를 냈습니다.

### Dilation and Erosion

#### Dilation

Dilation과 Erosion 에서도 필터가 입력 이미지를 훑고 지나갑니다. 다만, 데이터를 압축하는 것이 아니라, 주변 데이터를 인식하고 이에 따라 해당 픽셀의 값을 변화시킵니다.

말보다는 그림이죠. 그림으로 살펴봅시다.

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-02-filters.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-02-filters.png)

Dilation/Erosion에 사용되는 필터 Structual element라고 말하며, 위와 같이 1/0으로 이루어져있고 Convolutional filter와 마찬가지로 다양한 모양을 갖습니다.

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-03-dilation-1.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-03-dilation-1.png)

필터가 입력 이미지의 모든 픽셀을 훑고 지나갑니다. 이 때 "1"과 Foreground 가 맞닿는 부분이 생기게 되면, 필터의 중심이 위치한 픽셀 값을 Foreground로 바꾸어줍니다.

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-04-dilation-2.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-04-dilation-2.png)

즉, 위와 같은 경우 모든 1값이 Foreground와 겹치지 않기 때문에 넘어갑니다.

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-05-dilation-3.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-05-dilation-3.png)

이 때는 필터의 하단에 위치한 1이 Foreground와 겹치기 때문에, (1,4) 픽셀을 Foreground로 바꿔줍니다.

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-06-dilation-4.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-06-dilation-4.png)

다음 과정도 마찬가지겠지요.

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-07-dilation-5.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-07-dilation-5.png)

모든 픽셀을 훑게 되면 위와 같이 이전의 이미지 (네이비)보다 확장된 출력 이미지를 얻게 됩니다.

#### Erosion

Dilation은 Foreground를 확장시키는 역할을 수행했습니다. Erosion은 이와 반대로 Foreground를 축소시키는 역할을 수행합니다.

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-08-erosion.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-08-erosion.png)

Dilation과 반대로 필터의 1과 Background가 맞닿는 부분이 있다면, 필터의 중심 픽셀을 Background로 할당합니다.

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-09-erosion.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-09-erosion.png)

결과적으로 Erosion을 거치면 Foreground의 테두리 부분이 축소되는 효과를 얻습니다.

### Mixture model of Dilation and Erosion

#### Opening

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-10-role-of-erosion.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-10-role-of-erosion.png)

Erosion은 Foreground를 축소시키기 때문에, 위와 같이 미세한 노이즈를 제거하는 역할을 합니다. 하지만, 타겟 ROI의 크기도 축소시킨다는 단점이 존재합니다.

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-11-opening.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-11-opening.png)

따라서 Erosion 후에, Dilation을 다시 해서 미세한 노이즈는 제거한 뒤, 타겟 ROI의 크기를 다시 키워주는 응용을 할 수 있습니다. 이를 Opening이라고 합니다.

#### Closing

Opening이 있다면 Closing도 있겠지요. Closing은 Dilation 후 Erosion을 하는 것이라고 짐작할 수 있습니다.

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-12-dilation.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-12-dilation.png)

Dilation을 하게 되면, Foreground가 확장되기 때문에 타겟 ROI의 안에 잡힌 작은 홀들을 매꿔주는 역할을 합니다. 하지만 타겟 ROI가 커지는 단점이 존재합니다.

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-13-closing.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-13-closing.png)

이 때 다시 Erosion을 해주면 타겟 ROI를 다시 축소시키면서, 가운데 홀을 매꿔주고, 심지어 일부 미세한 노이즈도 제거해주는 효과를 얻을 수 있습니다.

## Drawback of morphological processing

완벽해보이지만 Morphological processing 만으로는 아직 부족합니다.

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-14-drawback.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-14-drawback.png)

Thresholding과 Morphological processing을 거쳐 좌측의 이미지를 얻었지만, 여전히 커다란 노이즈들은 존재합니다. 이를 없애줄 필요가 있습니다.

---

> 다음 포스팅에서는 사용자의 도움을 받아 ROI영역만을 분리하는 **Region growing**을 정리해보고자 합니다

다음 포스팅: [ 작성중 ]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/ 작성중 /)