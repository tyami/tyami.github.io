---
title: "제목"
excerpt: "부제"

categories:
- Medical image analysis

tags:
- tag1
- tag2

toc: true
toc_sticky: true
toc_label: 

use_math: true
---

이전 포스팅: [Medical Image Analysis 7: Classification for MEDIA (5) Overcome small dataset]({{ site.url }}{{ site.baseurl }}/Medical%20image%20analysis/MEDIA-7-classification-for-medical-image-5-small-dataset/)

> 이전 포스팅에서는 Medical image dataset의 특징인 적은 샘플 수를 극복하기 위한 여러 방법들을 정리  
> 이번 포스팅에서는 ** 내용 **을 정리해보고자 합니다.


---

![./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-1-ideal-case.png]({{ site.url }}{{ site.baseurl }})/assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics/./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-1-ideal-case.png)

![./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-10-f1-score-for-multi-label.png]({{ site.url }}{{ site.baseurl }})/assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics/./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-10-f1-score-for-multi-label.png)

![./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-2-real-case.png]({{ site.url }}{{ site.baseurl }})/assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics/./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-2-real-case.png)

![./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-3-accuracy.png]({{ site.url }}{{ site.baseurl }})/assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics/./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-3-accuracy.png)

![./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-4-problem-of-accuracy.png]({{ site.url }}{{ site.baseurl }})/assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics/./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-4-problem-of-accuracy.png)

![./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-5-sensitivitiy-specificity-precision.png]({{ site.url }}{{ site.baseurl }})/assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics/./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-5-sensitivitiy-specificity-precision.png)

![./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-6-roc-curve.png]({{ site.url }}{{ site.baseurl }})/assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics/./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-6-roc-curve.png)

![./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-7-f1-score.png]({{ site.url }}{{ site.baseurl }})/assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics/./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-7-f1-score.png)

![./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-8-confuision-matrix.png]({{ site.url }}{{ site.baseurl }})/assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics/./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-8-confuision-matrix.png)

![./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-9-f1-score-for-multi-label.png]({{ site.url }}{{ site.baseurl }})/assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics/./assets/images/post/MEDIA/2020-10-29-medical-image-classification-6-evaluation-metrics\2020-10-29-medical-image-classification-6-evaluation-metrics-9-f1-score-for-multi-label.png)

---

> 다음 포스팅에서는 ** 내용 **을 정리해보고자 합니다

다음 포스팅: [ 작성중 ]({{ site.url }}{{ site.baseurl }}/_posts/ 작성중 /)