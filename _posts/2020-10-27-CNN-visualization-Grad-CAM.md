---
title: "CNN visualization: CAM and Grad-CAM 설명"
excerpt: "CNN 모델의 학습결과를 시각화하는 Weakly-supervised learning의 예시로 CAM과 Grad-CAM을 정리해봅니다"

categories:
- Deep learning

tags:
- Deep learning
- Convolutional neural network
- Visualization
- Class activation map

toc: true
toc_sticky: true
toc_label: "CNN visualization: CAM and Grad-CAM"

use_math: true
---

> 이번 포스팅에서는 **CNN 모델이 어느 곳을 보고 있는지**를 알려주는 weak supervised learning 알고리즘 (CAM, Grad-CAM)에 대해 정리해보고자 합니다.

![2020-10-27-cnn-visualization-grad-cam-1-visualization-example]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-1-visualization-example.png)

학습한 네트워크가 이미지를 개라고 판별할 때와 고양이라고 판별할 때, 각각 이미지에서 중요하게 생각하는 영역은 다를 것입니다. 이를 시각화해주는 알고리즘이 바로 Class Activation Map (CAM) 관련 알고리즘들입니다.

## Convolutional neural network (CNN)

CNN에 대한 설명은 [이전 포스팅]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/DL-5-classification-for-medical-image-3-CNN/)을 참고해주세요 !

## Weakly Supervised learning

![2020-10-27-cnn-visualization-grad-cam-1-weakly-supervised-learning-example2]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-1-weakly-supervised-learning-example2.png)

학습 이미지에 대한 라벨만 있는 상황에서, 이미지의 어느 곳을 보았는가? 를 알고 싶을 때, 또는 학습 bounding box만 있는 상황에서, 각 pixel에 대한 label을 알고 싶을 때가 있을 것입니다.

즉, 학습할 이미지에 대한 정보보다 예측해야할 정보가 더 디테일한 경우, 이를 Weakly supervised learning이라고 부릅니다.

![2020-10-27-cnn-visualization-grad-cam-1-weakly-supervised-learning-example1]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-1-weakly-supervised-learning-example1.png)

오늘 다룰 CAM도 마찬가지로 이미지에 대한 label만 갖고 CNN 모델을 학습했지만, 결과적으로 이미지의 어느 부분을 주로 보았는가?를 알 수 있다는 점에서 Weakly supervised learning이라고 할 수 있습니다.

## Class Activation Map (CAM)

### Paper (2016)

본격적으로 CAM을 공부해봅시다.

![2020-10-27-cnn-visualization-grad-cam-2-cam-paper]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-2-cam-paper.png)

CAM은 2016년 CVPR에서 발표되었습니다.

![2020-10-27-cnn-visualization-grad-cam-3-cam-gap]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-3-cam-gap.png)

CAM을 요약한 이미지입니다. 눈에 띄는 정보로는 GAP 이라는 레이어가 보입니다. 마지막 Convolutional layer와 Target class 정보간 weight를 통해 Class Activation Map을 계산한다는 것을 확인할 수 있습니다.

### CNN architecture (conventional VS CAM)

![2020-10-27-cnn-visualization-grad-cam-4-cnn-architecture]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-4-cnn-architecture.png)

일반적인 CNN 구조는 Convolutional layer가 몇 장 있고, Fully-connected layer가 따라 붙는 구조를 띕니다.

![2020-10-27-cnn-visualization-grad-cam-5-cnn-architecture-with-cam]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-5-cnn-architecture-with-cam.png)

하지만 CAM 계산을 위해서는 Convolutional layer 이후를 Global Average Pooling (GAP) 레이어로 바꾸어주어야 합니다.

### Global Average Pooling (GAP)

![2020-10-27-cnn-visualization-grad-cam-6-gap]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-6-gap.png)

Global Average Pooling (GAP) layer는 입력 이미지의 모든 값의 평균을 출력을 내어줍니다. GAP 레이어로 들어오는 입력 이미지 feature map의 depth가 4라고 하면, 총 4개 값으로 이루어진 벡터를 얻게되는 것이죠.

극단적인 dimension reduction이라고 볼 수도 있습니다.

### Fine tuning

![2020-10-27-cnn-visualization-grad-cam-7-gap-fcn]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-7-gap-fcn.png)

GAP가 끝나면 뒤에는 각 클래스로 연결되는 Fully-connected layer를 붙여 fine-tuning 시킵니다.

![2020-10-27-cnn-visualization-grad-cam-8-cam-definition-equation]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-8-cam-definition-equation.png)

이 때 클래스 \\(c\\)에 대한 CAM 이미지는 아래와 같이 계산할 수 있습니다.

\\[
L_{CAM}^c (i,j)=\sum_k w_k^c f_k(i,j)
\\]

- \\(f_k(i,j)\\): \\(k\\)번째 feature image (\\(i,j\\)는 x, y 축 좌표를 의미함)
- \\(w_k^c\\): \\(k\\)번째 feature image \\(f_k(i,j)\\)에서 class \\(c\\)로 가는 weight

![2020-10-27-cnn-visualization-grad-cam-9-cam-1]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-9-cam-1.png)

그림으로 나타내보자면 이렇게 됩니다.

각 feature map \\(f_k(i,j)\\)에 각 class에 대한 가중치 \\(w_k^c\\)를 곱해주면 heatmap을 feature map 개수 \\(k\\)만큼 얻을 수 있습니다.

![2020-10-27-cnn-visualization-grad-cam-10-cam-2]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-10-cam-2.png)

이 heatmap 이미지를 모두 pixel-wise sum을 해주면, 하나의 heatmap을 얻을 수 있는데, 이게 바로 CAM 입니다.

![2020-10-27-cnn-visualization-grad-cam-11-cam-summary]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-11-cam-summary.png)

전체 과정을 요약하면 이렇게 됩니다.

![2020-10-27-cnn-visualization-grad-cam-12-cam-other-case]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-12-cam-other-case.png)

CAM의 특징은 class마다 계산할 수 있다는 점 입니다. 즉, Dog class로 CAM을 계산할 경우에는, 다른 heatmap을 얻게됩니다.

### Reason for last conv layer

마지막 Convolutional layer에 붙이는 이유는 GAP을 붙이기 용이하다는 점도 있지만, 각 레이어가 나타내는 정보도 영향이 있습니다.

![2020-10-27-cnn-visualization-grad-cam-13-reason-fot-last-conv-layer]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-13-reason-fot-last-conv-layer.png)

CNN의 각 레이어는 처음에는 specific한 정보를, 뒤로 갈수록 broad한 범위의 정보를 갖습니다. 따라서 이미지 전체 중 특정 영역을 찾기 위한 레이어로 마지막레이어가 가장 더 적합하다고 말할 수 있습니다.

### Results

![2020-10-27-cnn-visualization-grad-cam-14-cam-result]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-14-cam-result.png)

CAM의 결과이미지입니다. 학습이 잘 일어난 경우 label을 잘 설명하는 것을 확인할 수 있습니다.

### Limitation of CAM

CAM은 간단히 계산할 수 있는 유용한 툴이지만, Global Average Pooling layer를 사용해야만 한다는 한계점을 갖습니다. GAP으로 대치하게되면 뒷부분을 다시 또 fine tuning 해야하며, 마지막 Convolutional layer에 대해서만 CAM을 추출할 수 있다는 점도 한계점이라고 말할 수 있습니다.

## Gradient-weighted CAM (Grad-CAM)

### Paper (2017)

![2020-10-27-cnn-visualization-grad-cam-15-grad-cam-paper]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-15-grad-cam-paper.png)

바로 다음 해 2017년 ICCV에서 Grad-CAM의 저자들은 이 문제를 Gradient를 이용해 해결했습니다.

### Gradient

먼저 Gradient의 특징에 대해 짚고 넘어갑시다.

![2020-10-27-cnn-visualization-grad-cam-16-gradient-1]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-16-gradient-1.png)

hidden layer가 없는 경우, 각 input 노드는 output 노드에 하나의 weight를 통해 영향을 줍니다. 또한 이 path를 통해 Gradient 가 역전파됩니다.

![2020-10-27-cnn-visualization-grad-cam-17-gradient-2]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-17-gradient-2.png)

hidden layer가 추가되면, 각 input 노드는 모든 hidden layer의 노드를 거쳐 output 노드에 영향을 줍니다. Gradient 또한 모든 hidden layer를 거쳐 역전파됩니다.

![2020-10-27-cnn-visualization-grad-cam-18-gradient-3]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-18-gradient-3.png)

즉, Gradient는 **Class \\(C\\)에 대해 Input \\(K\\)가 주는 영향력**이라고 말할 수 있습니다.

### CNN architecture for Grad-CAM

![2020-10-27-cnn-visualization-grad-cam-4-cnn-architecture]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-4-cnn-architecture.png)

Grad-CAM을 위해서는 별도의 CNN 구조를 따를 필요가 없습니다. 즉 GAP 레이어를 쓰지 않아도 된다는 말입니다 !

### Generalized CAM

Grad-CAM은 GAP을 쓸 필요가 없다는 점에서 일반화된 CAM이라고도 말할 수 있습니다.

CAM의 수식은
\\[
L_{CAM}^c (i,j)=\sum_k w_k^c f_k(i,j)
\\]
이었고

Grad-CAM의 수식은
\\[
L_{Grad-CAM}^c (i,j)=ReLU(\sum_k a_k^c f_k(i,j)) \\\\\\
a_k^c=\frac{1}{Z} \sum_i \sum_j \frac{\partial S^c}{\partial f_k(i,j)}
\\]
입니다.

두 식의 차이점은 ReLU 함수가 추가되었다는 점과 \\(w_k^c\\)가 \\(a_k^c\\)로 변경되었다는 점입니다.

\\(a_k^c\\)의 수식을 글로 풀어 설명해보면, \\(k\\)번째 feature map \\(f_k(i,j)\\)의 각 원소 \\(i,j\\)가 Output class \\(c\\)의 matmul값 \\(S^c\\)에 주는 영향력의 평균이라고 말할 수 있습니다.

즉, CAM에서는 weight으로 주었던 각 feature map의 가중치를, Gradient로 대신 주었다고 생각하면 됩니다.

![2020-10-27-cnn-visualization-grad-cam-19-grad-cam-1]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-19-grad-cam-1.png)

그림으로 표현하면 이렇게 되겠죠. Gradient의 픽셀별 평균값인 \\(a_k^c\\)를 각 feature map \\(f_k(i,j)\\)에 곱해 heatmap을 만듭니다.

![2020-10-27-cnn-visualization-grad-cam-20-grad-cam-summary]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-20-grad-cam-summary.png)

그리고 마찬가지로 pixel-wise sum을 한 후, ReLU 함수를 적용해 양의 가중치를 갖는 (중요하게 여기는) 부분을 골라내면 Grad-CAM이 됩니다.

### Result

![2020-10-27-cnn-visualization-grad-cam-21-grad-cam-result]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-27-CNN-visualization-Grad-CAM/2020-10-27-cnn-visualization-grad-cam-21-grad-cam-result.png)

Grad-CAM의 결과입니다. 사진 속 (b)는 Guided backpropagation이라는 방법인데, specific한 곳을 visualization 해주는 방법입니다. 따라서 local 한 특성을 보여주는 Grad-CAM과 Specific한 특성을 보여주는 Guided backpropagation을 pixel-wise multiplication하게되면, Local+Specific 특징을 모두 갖는 Guided Grad-CAM을 얻을 수 있습니다.

---

## Reference
- [Grad-CAM: 대선주자 얼굴 위치 추적기](https://jsideas.net/grad_cam/): 설명뿐 아니라 Grad-CAM과 CAM이 수식적으로 동일하다는 증명을 적어둔 포스트입니다. 추가로 Keras 구현도 되어있습니다.

- [고려대학교 DMQA 랩 세미나, 백인성 님 발표 자료](http://dmqm.korea.ac.kr/activity/seminar/274): 설명 이미지 만들 때 참고자료로 사용했습니다.
