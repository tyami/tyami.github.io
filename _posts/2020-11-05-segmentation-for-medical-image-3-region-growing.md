---
title: "Segmentation for MEDIA (3) Region growing & Wathershed algorithm"
excerpt: "Region growing과 Wathershed algorithm을 정리해봅니다"

categories:
- Medical image analysis

tags:
- Medical image analysis
- Lecture
- Segmentation
- Region growing
- Wathershed

toc: true
toc_sticky: true
toc_label: "Region growing & Wathershed algorithm"

use_math: true
---

이전 포스팅: [Segmentation for MEDIA (2) Morphological processing]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/segmentation-for-medical-image-2-morphological-processing/)

> 이전 포스팅에서는 Threshold 이후 후처리에 사용되는 **Morphological processing**을 정리했습니다  
> 이번 포스팅에서는 **사용자의 도움을 받아 ROI 영역을 분리해내는 Region growing과 Wathershed 알고리즘**을 정리해보고자 합니다.

## Drawback of thresholding

![2020-11-04-segmentation-for-medical-image-2-morphological-processing-14-drawback.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-04-segmentation-for-medical-image-2-morphological-processing/2020-11-04-segmentation-for-medical-image-2-morphological-processing-14-drawback.png)

먼저 이전에 정리했던 Thresholding, Morphological processing에서 출발합니다. 이 방법들은 ROI와 크기가 비슷한 노이즈들은 제거하지 못한다는 결함이 존재했습니다.

## Region growing

![2020-11-05-segmentation-for-medical-image-3-region-growing-1-intro.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-05-segmentation-for-medical-image-3-region-growing/2020-11-05-segmentation-for-medical-image-3-region-growing-1-intro.png)

Region growing에서는 특정 ROI만 추출할 수 있도록, 사용자의 도움을 받습니다. 즉, 사용자가 특정 영역을 선택하면, 그 영역과 인접한 Foreground만 골라내는 알고리즘입니다.

![2020-11-05-segmentation-for-medical-image-3-region-growing-2.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-05-segmentation-for-medical-image-3-region-growing/2020-11-05-segmentation-for-medical-image-3-region-growing-2.png)

사용자가 특정 영역을 선택하면, 해당 영역을 시작으로 주변부를 스캔하면서 반복적으로 intensity를 비교하여 Foregrond 영역을 골라내면, 인접하지 않은 foreground (noise)들은 자동으로 탈락하게 됩니다.

### Region growing in brain image

![2020-11-05-segmentation-for-medical-image-3-region-growing-3.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-05-segmentation-for-medical-image-3-region-growing/2020-11-05-segmentation-for-medical-image-3-region-growing-3.png)

뇌 이미지에서는 이렇게 된다고 합니다.

### Region growing in RGB image

![2020-11-05-segmentation-for-medical-image-3-region-growing-4-rgb.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-05-segmentation-for-medical-image-3-region-growing/2020-11-05-segmentation-for-medical-image-3-region-growing-4-rgb.png)

흑백 이미지뿐 아니라, 컬러 이미지에도 Region growing 알고리즘이 활용됩니다.

### Drawback of region growing

사용자의 도움을 받기 때문에, Region growing도 결함을 가집니다.

![2020-11-05-segmentation-for-medical-image-3-region-growing-5-drawback.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-05-segmentation-for-medical-image-3-region-growing/2020-11-05-segmentation-for-medical-image-3-region-growing-5-drawback.png)

위와 같은 pathology 데이터의 경우, 모든 셀들을 클릭하기에는 너무 시간과 노동이 많이 든다는 단점이 있습니다.

## Watershed algorithm

Watershed 알고리즘이 이 때 사용될 수 있습니다. Watershed 알고리즘을 간략하게 설명하면 아래와 같습니다.

![2020-11-05-segmentation-for-medical-image-3-region-growing-6-watershed.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-05-segmentation-for-medical-image-3-region-growing/2020-11-05-segmentation-for-medical-image-3-region-growing-6-watershed.png)

(좌) Watershed 알고리즘은 위와 같이 Intensity 값을 바탕으로 지형을 그립니다. 그러면 여러 개의 웅덩이가 나오게 됩니다. 이제 이 지형에 물을 채운다고 생각할 때, 각 웅덩이의 경계점에 댐을 세우면, 웅덩이끼리 물이 섞이지 않습니다. 결과적으로 지형도의 맨 꼭대기까지 물이 차오르게 되면, 여러 개의 분리된 웅덩이가 형성이 됩니다 (segmentation)

(우) 이 경우 모든 웅덩이를 독립적으로 만들지만, 특정 웅덩이를 marker로 표시하면, marker가 표시된 웅덩이에만 댐을 설치합니다. 즉, marker가 없는 웅덩이들은 근처의 웅덩이로 통합됩니다.

![2020-11-05-segmentation-for-medical-image-3-region-growing-7-watershed-2D.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-11-05-segmentation-for-medical-image-3-region-growing/2020-11-05-segmentation-for-medical-image-3-region-growing-7-watershed-2D.png)

2차원의 이미지의 경우, 위와 같이 지형도를 두 개 만든다고 생각하면 됩니다.

---

> 다음 포스팅에서는 **Graph 모델을 이용한 segmentation 방법**을 정리해보고자 합니다

다음 포스팅: [Segmentation for MEDIA (4) Graph model-based segmentation]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/segmentation-for-medical-image-4-graph-model/)