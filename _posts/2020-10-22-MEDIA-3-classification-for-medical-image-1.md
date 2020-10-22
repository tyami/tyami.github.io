---
title: "Medical Imaging Analysis 3: Classification for MEDIA (1)"
excerpt: "의료영상을 위한 Classification 방법을 정리해봅시다 (1)"

categories:
- Medical Imaging Analysis

tags:
- Medical Imaging Analysis
- Lecture

toc: true
toc_sticky: true
toc_label: "Classification for MEDIA (1)"

use_math: true
---

이전 포스팅: [Medical Imaging Analysis 2: Medical Image acquisition methods](https://tyami.github.io/medical%20imaging%20analysis/MEDIA-2-medical-image-acquisition/)

> 이전 포스팅에서는 여러 종류의 의료영상 측정 방식에 대한 원리를 정리했습니다.  
> 이번 포스팅에서는 Medical Imaging Analysis 중 Classification에 대해 정리해보도록 하겠습니다.

Classification for MEDIA

## 
![2020-10-23-medical-image-classifcation-1-1-medical-image-properties]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-1-medical-image-properties.png)

![2020-10-23-medical-image-classifcation-1-2-demographic-score]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-2-demographic-score.png)

![2020-10-23-medical-image-classifcation-1-3-large-size]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-3-large-size.png)

![2020-10-23-medical-image-classifcation-1-4-what-to-learn-today]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-4-what-to-learn-today.png)

![2020-10-23-medical-image-classifcation-1-5-linear-regression]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-5-linear-regression.png)

![2020-10-23-medical-image-classifcation-1-6-gradient]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-6-gradient.png)

![2020-10-23-medical-image-classifcation-1-6-learning-rate]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-6-learning-rate.png)

![2020-10-23-medical-image-classifcation-1-7-class-y]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-7-class-y.png)

![2020-10-23-medical-image-classifcation-1-8-logistic-function]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-8-logistic-function.png)

![2020-10-23-medical-image-classifcation-1-9-logistic-regression]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-9-logistic-regression.png)

![2020-10-23-medical-image-classifcation-1-10-neural-network]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-10-neural-network.png)

![2020-10-23-medical-image-classifcation-1-11-deep-neural-network]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-11-deep-neural-network.png)

![2020-10-23-medical-image-classifcation-1-12-activation-functions]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-12-activation-functions.png)

![2020-10-23-medical-image-classifcation-1-13-image-classification]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-13-image-classification.png)

![2020-10-23-medical-image-classifcation-1-14-medical-image-classification]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-14-medical-image-classification.png)


![2020-10-23-medical-image-classifcation-1-15-feature-extraction]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-15-feature-extraction.png)

![2020-10-23-medical-image-classifcation-1-16-feature-extraction-classifier]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-16-feature-extraction-classifier.png)

![2020-10-23-medical-image-classifcation-1-17-classification-with-demographic-score]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-17-classification-with-demographic-score.png)

![2020-10-23-medical-image-classifcation-1-18-feature-normalizer]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-18-feature-normalizer.png)

![2020-10-23-medical-image-classifcation-1-19-example1-test]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-19-example1-test.png)

![2020-10-23-medical-image-classifcation-1-19-example1-training]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-19-example1-training.png)

![2020-10-23-medical-image-classifcation-1-20-example2-training]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-23-medical-image-classifcation-1/2020-10-23-medical-image-classifcation-1-20-example2-training.png)

---