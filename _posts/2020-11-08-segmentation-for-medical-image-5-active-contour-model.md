---
title: "Segmentation for MEDIA (5) Active contour model"
excerpt: "Shape update 기반의 active contour model을 정리해봅니다"

categories:
  - Medical image analysis

tags:
  - Medical image analysis
  - Lecture
  - Segmentation
  - Active contour model

toc: true
toc_sticky: true
toc_label: "Active contour model"

use_math: true
---

이전 포스팅: [Segmentation for MEDIA (4) Graph model-based segmentation]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/segmentation-for-medical-image-4-graph-model/)

> 이전 포스팅에서는 **Graph model-based segmentation**을 정리했습니다  
> 이번 포스팅에서는 **Active contour model**을 정리해보고자 합니다.

## Thresholding, Region growing, Graph model

앞의 포스팅에서 정리한 Thresholding (+Morphological processing), Region growing, Graph model 등 알고리즘들은 주어진 영상이미지의 pixel 정보 또는 인접 픽셀들의 label 정보를 바탕으로 segmentation을 진행했습니다. 즉, 이 방법들은 **사용자의 Domain knowledge를 활용하지 않는 순수한 영상처리 기법들**입니다.

![2020-11-08-segmentation-for-medical-image-5-active-contour-model-01-comparison.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-08-segmentation-for-medical-image-5-active-contour-model/2020-11-08-segmentation-for-medical-image-5-active-contour-model-01-comparison.png)

반면 이제부터 정리할 몇가지 segmentation 모델은 **Domain knowledge를 활용**합니다. 즉, Foreground의 형태에 대한 정보를 이미 알고 있는 상황에서 해당 Foreground를 효과적으로 분리해내는 방법을 고안했습니다.

# Active contour model

![2020-11-08-segmentation-for-medical-image-5-active-contour-model-02.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-08-segmentation-for-medical-image-5-active-contour-model/2020-11-08-segmentation-for-medical-image-5-active-contour-model-02.png)

Active contour model은 foreground의 경계 (contour)를 점차 업데이트 하면서 segmentation을 수행합니다.

![2020-11-08-segmentation-for-medical-image-5-active-contour-model-03.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-08-segmentation-for-medical-image-5-active-contour-model/2020-11-08-segmentation-for-medical-image-5-active-contour-model-03.png)

Active contour model에서는 여러 점 \\(v_s=(x_s, y_s)\\)의 위치를 점차 업데이트해나가면서 점들의 집합인 contour의 Energy \\(E=\int E(v_s)ds\\)를 최소화하는 contour를 찾는 것을 목표로 합니다. 즉, 앞서 정리했던 Graph model에서와 같이 Energy function을 최소화하는 문제로 Segmentation을 풉니다.

## Snake energy function

이 때 contour의 경계가 업데이트 되는 모습이 마치 뱀과 같이 구불구불하다고 해서 이 Energy function은 Snake energy function \\(E_snake\\)이라고 불립니다.

![2020-11-08-segmentation-for-medical-image-5-active-contour-model-04.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-08-segmentation-for-medical-image-5-active-contour-model/2020-11-08-segmentation-for-medical-image-5-active-contour-model-04.png)

Snake energy function은 그 때 그 때 사용자의 목적에 따라 다르게 정의할 수 있습니다.

일반적으로 위와 같이 External energy term 과 Internal energy term으로 나뉘어 정의되며, External/internal energy term 은 세부 energy term 으로 나뉩니다.

Snake energy function을 구성하는 여러 energy term 들을 정리해봅시다.

### External energy term

**External energy function은 영상의 특징을 고려해 정의한 energy function입니다.** 무슨말이냐 하면, Foreground는 "경계에 특정한 값을 갖는다" 또는 "Background와의 경계가 확실하게 나타난다" 같은 특징을 가질 수 있습니다. 이를 활용하여 Snake energy function가 최소화시켜야할 term을 추가할 수 있습니다.

#### Line energy

![2020-11-08-segmentation-for-medical-image-5-active-contour-model-05.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-08-segmentation-for-medical-image-5-active-contour-model/2020-11-08-segmentation-for-medical-image-5-active-contour-model-05.png)

Line energy term은 **Foreground의 boundary의 line에서 구분되는 값이 있을 것이다**라는 것을 가정하여 energy function을 정의합니다.

예를 들어 Line energy term 을 \\(E*{line}=I(x)\\)로 정의하면, contour가 black line (\\(I(x)=0\\))을 따라 위치해 있을 때 \\(E*{line}\\)이 가장 작은 값을 나타낼 것입니다.

![2020-11-08-segmentation-for-medical-image-5-active-contour-model-06.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-08-segmentation-for-medical-image-5-active-contour-model/2020-11-08-segmentation-for-medical-image-5-active-contour-model-06.png)

반면, Line energy term을 \\(E*{line}=-I(x)\\)로 정의하면, 위와 반대로 contour가 white line (\\(I(x)=255\\))를 따라 위치해 있을 때 \\(E*{line}\\)이 가장 작은 값을 나타낼 것입니다.

#### Edge energy

![2020-11-08-segmentation-for-medical-image-5-active-contour-model-07.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-08-segmentation-for-medical-image-5-active-contour-model/2020-11-08-segmentation-for-medical-image-5-active-contour-model-07.png)

또 다른 가정으로 **경계가 뚜렷한 곳에서 Boundary가 생긴다**라는 가정을 띄고 정의한 Energy term을 Edge energy term이라고 합니다.

\\[
E_{edge}=-\lvert \nabla I(x,y) \rvert^2
\\]

Energy function을 위와 같이 정의하면, \\(x\\)와 \\(y\\) 사이의 변화량의 절대값이 클 수록, energy function이 작은 값을 나타냅니다.

### Internal energy term

External energy term이 영상에서의 특징을 고려한 것이라면, **Internal energy term은 타겟 ROI의 shape 정보 (Prior)를 바탕으로 정의한 energy term** 입니다.

대표적인 예시로 Elastic energy term과 Bending energy term을 정리해봅시다.

#### Elastic energy term

Elastic energy term은 타겟 ROI의 shape이 굴곡이 별로 없다는 것을 알고 있을 때 효과적인 energy term 입니다.

![2020-11-08-segmentation-for-medical-image-5-active-contour-model-08.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-08-segmentation-for-medical-image-5-active-contour-model/2020-11-08-segmentation-for-medical-image-5-active-contour-model-08.png)

위 식과 같이 각 픽셀값 \\(v_s\\)을 1차미분한 \\(V_s\\) 에 가중치를 곱한 값의 합으로 정의합니다.

그러면 Elastic energy function을 최소화하기 위해, 우측 그림처럼 굴곡진 부분이 평평하게 업데이트되는 효과를 얻을 수 있습니다.

#### Bending energy term

Bending energy term은 타겟 ORI의 shape에 뾰족한 부분이 별로 없다는 것을 알고 있을 때 효과적인 energy term 입니다.

![2020-11-08-segmentation-for-medical-image-5-active-contour-model-09.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-08-segmentation-for-medical-image-5-active-contour-model/2020-11-08-segmentation-for-medical-image-5-active-contour-model-09.png)

Elastic energy term에서의 1차 미분값 \\(V*s\\)를 다시 한번 더 미분한 2차 미분값 \\(V*{ss}\\)에 가중치를 곱한 값의 합으로 정의합니다.

이번에는 뾰족한 부분이 뭉툭해지도록 업데이트되는 효과를 확인할 수 있습니다.

### Summary

![2020-11-08-segmentation-for-medical-image-5-active-contour-model-10.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-08-segmentation-for-medical-image-5-active-contour-model/2020-11-08-segmentation-for-medical-image-5-active-contour-model-10.png)

따라서 Snake energy function의 기본적인 전체 모습은 위와 같이 Line energy, Edge energy, Elastic energy, Bending energy term을 모두 합친 형태가 됩니다.

이외에 영상의 특징을 고려해서 추가의 term을 붙인다고도 합니다.

---

> 다음 포스팅에서는 **Atlas-based 알고리즘**을 정리해보고자 합니다

다음 포스팅: [Segmentation for MEDIA (6) Atlas-based methods and label fusion]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/segmentation-for-medical-image-6-atlas-based-methods/)
