---
title: "Introduction to MEDIA"
excerpt: "Medical Image Analysis 1: Introduction to MEDIA"

categories:
- Medical Image Analysis

tags:
- Medical Image Analysis
- Lecture
- Classification

toc: true
toc_sticky: true
toc_label: "Introduction to MEDIA"

use_math: true
---

Medical Image Analysis (MEDIA) 에 대해 공부하면서, 관련 내용을 정리하고자 카테고리를 추가했습니다.

정리된 자료는 edwith에서 제공하는 DGIST 박상현교수님의 [컴퓨터비전, 머신러닝, 딥러닝을 이용한 의료영상분석](https://www.edwith.org/medical-20200327/home) 강의를 토대로 작성하였습니다.

Classification, Segmentation, Enhancement, Registration을 세부 주제로 구성됩니다.

1. Introduction to MEDIA (본 글)
2. [Medical Image acquisition methods]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/MEDIA-2-medical-image-acquisition/)
3. [Classification for MEDIA (1)]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/MEDIA-3-classification-for-medical-image-1/)
4. [Classification for MEDIA (2) Deep neural network]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/MEDIA-4-classification-for-medical-image-2-DNN/)
5. [Classification for MEDIA (3) Convolutional neural network]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/MEDIA-5-classification-for-medical-image-3-CNN/)
6. [Classification for MEDIA (4) Advanced CNN models]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/2020-10-26-MEDIA-6-classification-for-medical-image-4-advanced-CNN/)
7. [Classification for MEDIA (5) Overcome small dataset]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/MEDIA-7-classification-for-medical-image-5-small-dataset/)
8. [Classification for MEDIA (6) Evaluation metrics for classification (분류모델 평가 지표)]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/MEDIA-8-classification-for-medical-image-6-evaluation-metrics/)
9. [Classification for MEDIA (7) Feature selection and extraction]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/classification-for-medical-image-7-feature-selection-extraction/)
10. [Segmentation for MEDIA (1) Otsu threshold]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/segmentation-for-medical-image-1-otsu-threshold/)
11. [Segmentation for MEDIA (2) Morphological processing]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/segmentation-for-medical-image-2-morphological-processing/)

## MEDIA Overview

본 강의는 Medical Image Analysis (MEDIA)에 대해서 다룹니다.

![2020-10-21-introduction-to-MEDIA-1-range]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-21-introduction-to-MEDIA/2020-10-21-introduction-to-MEDIA-1-range.png)

MEDIA는 Computer vision과 Machine learning 분야와 접점이 있습니다. Computer vision에서는 주로 2D 이미지를 다루는 반면, MEDIA에서는 3D image를 주로 다룬다는 특징이 있습니다. Deep learning을 포함한 Machine learning 은 연구 방법으로 많이 사용됩니다.

## Opportunities for AI in MEDIA

![2020-10-21-introduction-to-MEDIA-2-opportunities-for-AI]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-21-introduction-to-MEDIA/2020-10-21-introduction-to-MEDIA-2-opportunities-for-AI.png)

MEDIA에서 Artificial intelligence (AI)는 다양한 부분에서 활용될 수 있습니다.

1. 가장 먼저 의료영상 취득 시 Reconstruction 기법에 활용될 수 있습니다. 주로 CT (Computed Tomography)나 Positron Emission Tomography (PET)에서 활용될 수 있습니다.
2. Image labelling에 사용될 수 있습니다.
3. 수집 및 라벨링된 데이터의 분석에 사용될 수 있습니다.
4. 설명을 위한 자료로 사용될 수 있습니다.


### Research examples

![2020-10-21-introduction-to-MEDIA-3-research-examples-GE]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-21-introduction-to-MEDIA/2020-10-21-introduction-to-MEDIA-3-research-examples-GE.png)

Rconstruction 연구의 예로는 CT 영상 촬영 시, 딥러닝을 활용하여 노출되는 방사선양을 줄이면서도 이미지 성능을 높이기 위한 연구가 있습니다.

![2020-10-21-introduction-to-MEDIA-4-research-examples-Philips]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-21-introduction-to-MEDIA/2020-10-21-introduction-to-MEDIA-4-research-examples-Philips.png)

또 다른 예로, 현미경 이미지를 디지털화하고 이를 머신러닝을 이용해 분석하는 연구도 있습니다.

![2020-10-21-introduction-to-MEDIA-5-research-examples-CheXNet]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-21-introduction-to-MEDIA/2020-10-21-introduction-to-MEDIA-5-research-examples-CheXNet.png)

나아가 X-ray 등 의료영상을 바탕으로 의료질환을 발견하여 의사의 판독에 도움을 주는 연구도 존재합니다.

### Sub-categories related to computer vision

앞서 말한 바와 같이 MEDIA는 Computer vision과 밀접한 관련이 있습니다.

MEDIA의 연구 분야를 크게 네 부분으로 나눠볼 수 있습니다.

#### Classification

![2020-10-21-introduction-to-MEDIA-6-classification]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-21-introduction-to-MEDIA/2020-10-21-introduction-to-MEDIA-6-classification.png)

개와 고양이를 구분짓는 분류문제처럼, 일반인과 환자를 구분짓는 분류 문제를 해결하는데 일조할 수 있습니다.

#### Segmentation

![2020-10-21-introduction-to-MEDIA-7-segmentation]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-21-introduction-to-MEDIA/2020-10-21-introduction-to-MEDIA-7-segmentation.png)

자율주행 연구에서 자동차, 사람, 사물 등을 구분짓듯이 의료영상 이미지 내에서 다양한 조직, 세포 등을 구분짓는 일을 할 수도 있습니다.

#### Enhancement

![2020-10-21-introduction-to-MEDIA-8-enhancement]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-21-introduction-to-MEDIA/2020-10-21-introduction-to-MEDIA-8-enhancement.png)

이미지의 해상도를 높이는 Super resolution을 접목시켜, 측정된 의료영상 이미지의 해상도를 높이는 연구가 될 수도 있습니다.

#### Registration

![2020-10-21-introduction-to-MEDIA-9-registration]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-21-introduction-to-MEDIA/2020-10-21-introduction-to-MEDIA-9-registration.png)

서로 다른 시점에 촬영된 여러 영상 이미지를 동일한 포맷으로 맞춰주는 작업에도 활용될 수 있습니다.

### Algorithms by subcategories

![2020-10-21-introduction-to-MEDIA-10-contents]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-21-introduction-to-MEDIA/2020-10-21-introduction-to-MEDIA-10-contents.png)

각각 sub-category에서 다룰 알고리즘들입니다.

---

다음 포스팅: [Medical Image acquisition methods]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/MEDIA-2-medical-image-acquisition/)