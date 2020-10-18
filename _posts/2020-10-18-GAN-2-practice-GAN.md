---
title: "Generative Adversarial Nets (GAN) 2: GAN을 PyTorch로 구현해보기"
excerpt: "PyTorch로 GAN을 구현해봅시다"

categories:
- Deep learning

tags:
- Deep learning
- GAN

toc: true
toc_sticky: true
toc_label: "GAN 구현 (PyTorch)"

use_math: true
---

이전 글 보기: [Generative Adversarial Nets (GAN) 1: GAN과 DCGAN 설명](https://tyami.github.io/deep%20learning/GAN-1-theory-GAN-DCGAN/)

> 이전 포스팅에서는 Generative Adversarial Network의 기본 알고리즘인 GAN과 DCGAN을 정리했습니다.  
> 이번 포스팅에서는 PyTorch를 이용해 GAN 모델을 직접 구현해보도록 하겠습니다.
 
## 구성

본 구현에서는 단순한 이미지셋인 MNIST를 사용하여 GAN을 학습시키고자 합니다. GAN 모델은 Generative model \\(G\\)과 Discriminative model \\(D\\)를 번갈아가면서 업데이트해나갑니다.

1. Libraries
2. Dataloader
3. Generative model \\(G\\)
4. Discriminative model \\(D\\)
5. Model train
6. Visualization


## 1. Libraries

GAN 구현에 사용될 라이브러리들을 불러옵니다.

```python
import numpy as np
import pandas as pd

import matplotlib
import matplotlib.pyplot as plt

import torch
import torch.nn as nn
import torch.optim as optim
from torch.autograd import Variable

import torchvision
import torchvision.utils as utils
import torchvision.datasets as dsets
import torchvision.transforms as transforms
```

버전을 확인합니다.

```python
print('numpy: ' + np.__version__)
print('pandas: ' + pd.__version__)
print('matlotlib: ' + matplotlib.__version__)
print('torch: ' + torch.__version__)
print('torchvision: ' + torchvision.__version__)
```
> numpy: 1.16.0  
> pandas: 0.25.3  
> matlotlib: 3.0.3  
> torch: 1.5.1  
> torchvision: 0.6.1

## 2. Dataloader

## 3. Generative model \\(G\\)

## 4. Discriminative model \\(D\\)

## 5. Model train

## 6. Visualization

> 다음 포스팅은 또 뭐가 있을까요?

다음 글 보기: 미정
