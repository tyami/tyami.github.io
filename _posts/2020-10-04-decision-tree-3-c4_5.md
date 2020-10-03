---
title: "의사결정 나무 (Decision Tree) C4.5 알고리즘 설명"
excerpt: "의사결정 나무의 기본 알고리즘 중 하나인 C4.5 를 공부해봅시다"

categories:
- Machine learning

tags:
- Machine learning
- Decision Tree
- Algorithm

toc: true
toc_sticky: true
toc_label: "Decision Tree C4.5 알고리즘"

use_math: true
---

이전 글 보기: [의사결정 나무 (Decision Tree) ID3 알고리즘 설명](https://tyami.github.io/machine%20learning/decision-tree-2-ID3/)

이전 포스팅에서는 의사결정 나무의 가장 기본적인 알고리즘인 ID3 알고리즘을 예시를 통해 정리했습니다.
이번 포스팅에서 소개할 C4.5 알고리즘은 ID3알고리즘과 동일하게 엔트로피를 불순도(Impurity)로 사용하는 알고리즘입니다.
 
## C4.5 알고리즘
C4.5 알고리즘이 ID3알고리즘에 비해 개선된 점은 아래와 같이 요약할 수 있습니다.

- 정교한 불순도 지표 (Information gain ratio) 활용
- 범주형 변수 뿐 아니라 연속형 변수를 사용 가능
- 결측치가 포함된 데이터도 사용 가능
- 과적합을 방지하기 위한 가지치기

4가지 개선점이 어떻게 적용되었는지 하나씩 살펴보도록 하겠습니다.

---
### 개선점 1: 정교한 불순도 지표

ID3 알고리즘에서는 분기 전후의 엔트로피를 기반으로 Information Gain (IG)을 계산하고, 이를 최대화하는 방향으로 지표를 정했습니다.
하지만 이 알고리즘에는 한계점이 존재합니다.

#### Information Gain의 한계점

![IG의 한계점 - 예시 1]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-04-IG-limitation-example1.png)

*Windy*라는 지표로 분기했을 때 데이터가 위와 같이 분기된다고 가정해봅시다. 이 경우의 Information Gain은 0.5568입니다.
\begin{aligned}
Information Gain&=H(7,3) - (\frac{6}{10}H(6,0)+\frac{4}{10}H(1,3)) \\\\\\
&=0.8813-(\frac{6}{10} \times 0+\frac{4}{10} \times 0.8113) \\\\\\
&=0.5568
\end{aligned}

![IG의 한계점 - 예시 2]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-04-IG-limitation-example2.png)

다른 지표인 *With whom*이라는 지표로 분기했을 때는 Information Gain이 **0.6813**이 나옵니다.
\begin{aligned}
Information Gain&=H(7,3) - (\frac{1}{10}H(1,0)+\frac{1}{10}H(1,0)+...+\frac{1}{10}H(0,1)+\frac{2}{10}H(1,1)) \\\\\\
&=0.8813-(\frac{1}{10} \times 0+...+\frac{2}{10} \times 1) \\\\\\
&=0.6813
\end{aligned}

두 경우를 비교했을 때, With whom? 지표가 더 효과적인 지표라고 말할 수 있을까요? With whom 지표는 데이터를 너무 잘게 분해하기 때문에 좋은 지표라고 볼 수 없습니다.

#### Information Gain Ratio (GR)
C4.5 알고리즘에서는 이와 같은 문제를 해결하기 위해, **Information Gain Ratio**라는 지표를 사용하였습니다.
\[ Information Gain Ratio = \frac{IG}{IV} \]

\\(IG\\)는 ID3 알고리즘에서의 Information Gain을 의미하며, \\(IV\\)는 **Intrinsic Value**를 의미합니다.

![Information Gain Ratio overview]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-04-IGR-overview.png)

특정 지표로 분기했을 때 생성되는 가지의 수를 \\(N\\)이라고 하고, \\(i\\)번째 가지에 해당하는 확률을 \\(p_i\\)라고할 때, Intrinsic Value는 아래와 같습니다.
\[ Intrinsic Value=IV=-\sum_i^N p_i log_2 p_i \]

---

앞서 예시로 든 두 가지 지표 *Windy*, *With whom*에 대한 Information Gain Ratio를 계산하여 차이점을 비교해봅시다.

![IG의 한계점 - 예시 1]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-04-IG-limitation-example1.png)

*Windy* 지표에 대한 Information Gain Ratio를 계산해보면, 첫 번째 경우는 **0.5739**라는 값을 얻을 수 있습니다.
\begin{aligned}
Information Gain Ratio&=IG/IV \\\\\\
&=\frac{0.5568}{-(\frac{6}{10}log_2 \frac{6}{10} + \frac{4}{10}log_2 \frac{4}{10})} \\\\\\
&=\frac{0.5568}{0.9701} \\\\\\
&=0.5739
\end{aligned}

![IG의 한계점 - 예시 2]({{ site.url }}{{ site.baseurl }}/assets/images/post/ML/2020-10-04-IG-limitation-example2.png)

동일한 방법으로 *With whom*에 대한 GR을 계산하면 **0.2182**라는 값을 얻습니다.
\begin{aligned}
Information Gain Ratio&=IG/IV \\\\\\
&=\frac{0.6813}{3.1219} \\\\\\
&=0.2182
\end{aligned}

아까와는 달리 Information Gain Ratio를 사용할 경우, Windy지표가 With whom 지표보다 더 좋은 지표임을 확인할 수 있습니다.

### 개선점 2: 연속형 변수를 사용 가능
*작성 중입니다*

---
이번 포스팅에서는 ID3알고리즘에 비해 C4.5 알고리즘에서 개선된 점들을 알아보았습니다.
다음 포스팅에서는 엔트로피 기반 불순도가 아닌 지니 계수를 불순도를 사용하는 CART (Classification And Regression Tree) 알고리즘에 대해 알아보도록 하겠습니다.
