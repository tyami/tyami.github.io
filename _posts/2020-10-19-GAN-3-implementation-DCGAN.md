---
title: "Generative Adversarial Nets (GAN) 3: DCGAN을 PyTorch로 구현해보기"
excerpt: "PyTorch로 DCGAN을 구현해봅시다"

categories:
- Deep learning

tags:
- Deep learning
- GAN
- Implementation

toc: true
toc_sticky: true
toc_label: "DCGAN 구현 (PyTorch)"

use_math: true
---

이전 글 보기: [Generative Adversarial Nets (GAN) 2: GAN을 PyTorch로 구현해보기]({{ site.url }}{{ site.baseurl }}/deep%20learning/GAN-2-implementation-GAN/)

> 이전 포스팅에서는 PyTorch를 이용해 GAN 모델을 직접 구현해보았습니다.
> 이번 포스팅에서는 DCGAN 모델을 구현해봅시다 !  

> **왜인지는 모르겠지만 학습이 안정적으로 이루어지지 않았습니다. 추후 문제를 찾아 수정하도록 하겠습니다.**  
> 전체적으로 [PyTorch DCGAN Tutorial](https://pytorch.org/tutorials/beginner/dcgan_faces_tutorial.html)을 참고하였습니다. 코드를 그대로 복붙하기보다는, 내용을 이해하고 이전에 작성한 GAN 코드 기반으로 다시 작성하는 식으로 작성해보았는데, 이 때문에 학습이 안 된걸수도 있겠네요 😒

## DCGAN

DCGAN는 네트워크를 Convolutional layer로 구성한 GAN입니다. 자세한 설명은 [이전 포스팅]({{ site.url }}{{ site.baseurl }}/deep%20learning/GAN-1-theory-GAN-DCGAN/)에 있습니다.

## 구성

본래 DCGAN에서는 CelebA 이미지셋을 사용해서 모델을 학습시켰습니다. 하지만 CelebA 데이터셋 다운로드 오류로, 이번 구현에서는 MNIST 또는 CIFAR-10 이미지를 사용하여 모델을 학습시켜보도록 하겠습니다 (추후 업데이트 예정).

본 포스트의 구성은 아래와 같습니다. 전체적인 구성은 GAN과 동일합니다.

1. Load libraries
2. MNIST/CIFAR-10 dataset download
3. Random sample \\(z\\) from normal distribution
4. Generative model \\(G\\)
5. Disciminative model \\(D\\)
6. Train model \\(G\\) and \\(D\\)
   1. Initialize model \\(G\\) and \\(D\\)
   2. Loss functions & Optimizers
   3. Train models
   4. Save model weights
7. Visualization (Interpolation)

전체 코드는 [Github](https://github.com/tyami/implementation-deep-learning/blob/master/code/2-DCGAN-PyTorch.ipynb)에서 볼 수 있습니다.

## 구현

### 1. Load libraries

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

본 구현에 사용된 라이브러리 환경은 아래와 같습니다 (python=3.5)
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

결과 재현을 위해 random seed를 설정합니다.
```python
import random

# Set random seed for reproducibility
manualSeed = 2020
#manualSeed = random.randint(1, 10000) # use if you want new results
print("Random Seed: ", manualSeed)
random.seed(manualSeed)
torch.manual_seed(manualSeed);
```

GPU를 사용할지 선택합니다. 사용 가능한 GPU가 있다면 `device` 변수에 *cuda*가, 없다면 *cpu*가 표시됩니다.
```python
is_cuda = torch.cuda.is_available()
device = torch.device('cuda' if is_cuda else 'cpu')

print(device)
```
> cuda

### 2. MNIST dataset download

하이퍼파라미터 `batch_size`와 `image_size`를 설정합니다.
- `batch_size`: 배치 사이즈를 설정합니다.
- `image_size`: 출력물 이미지 크기를 설정합니다.

```python
# hyper-parameter
image_size = 64
batch_size = 64
```

(CIFAR-10 학습 시) CIFAR-10 전처리 모듈 정의 및 다운로드를 수행합니다.
```python
# standardizer
standardizer = transforms.Compose([
                                    transforms.Resize(image_size),
                                    transforms.ToTensor(),
                                    transforms.Normalize(mean=(0.5, 0.5, 0.5),
                                                        std=(0.5, 0.5, 0.5))])

# CIFAR_10 dataset
train_data = dsets.CIFAR10(root='../data/', train=True, transform=standardizer, download=True)
```

(MNIST 학습 시) MNIST 전처리 모듈 정의 및 다운로드를 수행합니다.
```python
# standardizer
standardizer = transforms.Compose([
                                    transforms.Resize(image_size),
                                    transforms.ToTensor(),
                                    transforms.Normalize(mean=0.5,
                                                        std=0.5)])

# CIFAR_10 dataset
train_data = dsets.MNIST(root='../data/', train=True, transform=standardizer, download=True)
```

`batch_size` 단위로 이미지를 로드하기 위한 dataloader를 정의합니다.
```python
# Data loader
train_data_loader = torch.utils.data.DataLoader(train_data, batch_size, shuffle=True)
```

제대로 로드가 되는지 확인하기 위해, 몇 개의 이미지를 시각화해봅니다.
```python
# function for visualization
def tc_imshow(img, lbl=""):
    if img.size(0) == 1:
        plt.imshow(img.squeeze(), cmap='gray')
    else:
        plt.imshow(np.transpose(img, (1, 2, 0)))
        
    plt.title(lbl)
    plt.axis('off')
```

```python
# visualize

# visualize
mini_batch_img, mini_batch_lbl = next(iter(train_data_loader))

plt.figure(figsize=(4,5))
for i in range(16):
    plt.subplot(4,4,i+1)
    tc_imshow(img=mini_batch_img[i] /2+0.5 ,
              lbl=train_data.classes[mini_batch_lbl[i].numpy()])
    plt.axis('off')
    
plt.savefig('../result/GAN/2-DCGAN/1-dataloader-example.png', dpi=300)
```
> ![GAN dataloader test]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-19-DCGAN-implementation/2020-10-19-DCGAN-implementation-1-dataloader-example.png)

### 3. Random sample \\(z\\) from uniform distribution

하이퍼파라미터 `dim_noise`를 정의합니다. `dim_noise`는 latent space \\(z\\)의 차원을 의미합니다.
```python
# hyper-parameter
dim_noise = 100
```

latent space \\(z\\)를 생성합니다. \\(z\\)는 **uniform distribution**으로부터 random sampling됩니다. **여기서 주목할 점은 `dim_noise`x1의 크기로 \\(z\\)가 생성되는 것이 아니라 `dim_noise`x1x1의 크기로 생성된다는 점입니다.**
```python
# Random sampling from uniform distribution
def random_sample_z_space(batch_size=1, dim_noise=100):
    return torch.rand(batch_size, dim_noise, 1, 1, device=device)
```

### 4. Generative model \\(G\\)

![DCGAN 구조]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-17-GAN-DCGAN-theory/2020-10-17-GAN-DCGAN-theory-10-DCGAN-G-model.png)

![DCGAN 규칙]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-17-GAN-DCGAN-theory/2020-10-17-GAN-DCGAN-theory-9-DCGAN-key-concept.png)

하이퍼파라미터 `dim_G_last2_channel`과 `dim_output`을 정의합니다.
- `dim_G_last2_channel`: 최종 출력 이미지 직전 레이어의 채널 수 (각 레이어 채널수의 기본 단위수로 사용됨)
- `dim_output`: 최종 출력 이미지의 채널 수 (MINST: 1, CIFAR-10: 3)

하이퍼파라미터 값에 따라 `dim_output`과 `img_shape` 변수가 정해집니다.

```python
# hyper-paremeters
dim_G_last2_channel = 64
dim_output = mini_batch_img.size(1)
```

DCGAN 저자의 논문에 따라 모델의 weight를 평균 0, 표준편차 0.02인 정규분포에서 샘플링하여 초기화해줍니다. 

> 의문: PyTorch Tutorial 을 참고하여 작성한 코드인데, 왜 BatchNorm layer들은 평균 1.0으로 초기화 시켰을까요?

```python
def initialize_weights(model):
    class_names = model.__class__.__name__
    
    if class_names.find('Conv') != -1:
        nn.init.normal_(model.weight.data, 0.0, 0.02)
        
    elif class_names.find('BatchNorm') != -1:
        nn.init.normal_(model.weight.data, 1.0, 0.02)
        nn.init.constant_(model.bias.data, 0)
```

Generator 구현체를 class로 정의합니다. 구현체로 정해주게 되면, 처음 선언될 때 `__init__(self)`이 실행되며, `z`를 인풋으로 받을 때, `forward(self, z)`가 실행됩니다.

일반적으로 `__init__(self)` 함수에서 네트워크 구조를 정의합니다. 네트워크 구조는 fully-connected layer가 아닌 convolutional layer로 구성합니다.

> 개인적으로 DCGAN 구현 코드들을 참고하며 가장 이해가 안 갔던 부분이 DCGAN 구조 중 맨 처음에 위치한 **Project and reshape** 구간이 \\(G\\) 모델 상에 없다는 점이었습니다. 100x1 vector가 아닌 100x1x1 matrix 형태로 \\(z\\) latent space를 샘플링하여 사용했기 때문에 이 과정이 삭제된 것으로 이해했습니다.

> 의문: ConvTranspose layer 들의 `kernel_size`, `padding` 옵션은 어떻게 정해진 건지가 궁금합니다. `stride` 같은 경우에는 그림에 표시되어있어서 확인할 수 있는데, `kernel_size`나 `padding` 옵션은 정보를 찾을 수가 없었습니다.

```python

class Generator(nn.Module):
    def __init__(self):
        super(Generator, self).__init__()

        self.model = nn.Sequential(
                    # 100x1 z 1D vector에서 2D vector로의 변환이 없는 이유: 애초에 z random sampling 시 1x1 matrix 형태로 sampling 하면 됨.
                    nn.ConvTranspose2d(in_channels=dim_noise, 
                                       out_channels=dim_G_last2_channel*8, 
                                       kernel_size=4, # 어떻게 정해진거지?
                                       stride=1,
                                       padding=0, # 어떻게 정해진거지?
                                       bias=False),
                    nn.BatchNorm2d(dim_G_last2_channel*8),
                    nn.ReLU(True),
            
                    nn.ConvTranspose2d(in_channels=dim_G_last2_channel*8, 
                                       out_channels=dim_G_last2_channel*4, 
                                       kernel_size=4, # 어떻게 정해진거지?
                                       stride=2,
                                       padding=1, # 어떻게 정해진거지?
                                       bias=False),
                    nn.BatchNorm2d(dim_G_last2_channel*4),
                    nn.ReLU(True),
            
                    nn.ConvTranspose2d(in_channels=dim_G_last2_channel*4, 
                                       out_channels=dim_G_last2_channel*2, 
                                       kernel_size=4, # 어떻게 정해진거지?
                                       stride=2,
                                       padding=1, # 어떻게 정해진거지?
                                       bias=False),
                    nn.BatchNorm2d(dim_G_last2_channel*2),
                    nn.ReLU(True),
            
                    nn.ConvTranspose2d(in_channels=dim_G_last2_channel*2, 
                                       out_channels=dim_G_last2_channel, 
                                       kernel_size=4, # 어떻게 정해진거지?
                                       stride=2,
                                       padding=1, # 어떻게 정해진거지?
                                       bias=False),
                    nn.BatchNorm2d(dim_G_last2_channel),
                    nn.ReLU(True),

                    nn.ConvTranspose2d(in_channels=dim_G_last2_channel, 
                                       out_channels=dim_output, 
                                       kernel_size=4, # 어떻게 정해진거지?
                                       stride=2,
                                       padding=1, # 어떻게 정해진거지?
                                       bias=False),
                    nn.Tanh()
        )
    def forward(self, z):
        img = self.model(z)
        
        return img
```

\\(G(z)\\)의 이미지를 그려봅니다. 아직 generator \\(G\\)가 학습되지 않은 상태이기 때문에, nosiy한 이미지만 출력됩니다.
```python
# visualize
utils.save_image(G(z)[:25].cpu().detach(), "../result/GAN/2-DCGAN/2-G(z).png", nrow=5, normalize=True)
```
> ![DCGAN G(z) test]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-19-DCGAN-implementation/2020-10-19-DCGAN-implementation-2-G(z).png)

### 5. Disciminative model \\(D\\)

Discriminative model \\(D\\)의 형태는 \\(G\\)를 거꾸로 뒤집은 형태입니다. \\(G\\)의 MLP 구조를 거꾸로 구성해줍니다. \\(D\\)의 출력 레이어는 입력으로 받은 이미지가 진짜인지 가짜인지 판단하는 sigmoid function을 이용합니다.
```python
class Discriminator(nn.Module):
    def __init__(self):
        super(Discriminator, self).__init__()
        
        self.model = nn.Sequential(
            nn.Conv2d(in_channels=dim_output,
                      out_channels=dim_G_last2_channel,
                      kernel_size=4,
                      stride=2,
                      padding=1,
                      bias=False),
            nn.BatchNorm2d(dim_G_last2_channel),
            nn.LeakyReLU(0.2, inplace=True),
            
            nn.Conv2d(in_channels=dim_G_last2_channel,
                      out_channels=dim_G_last2_channel*2,
                      kernel_size=4,
                      stride=2,
                      padding=1,
                      bias=False),
            nn.BatchNorm2d(dim_G_last2_channel*2),
            nn.LeakyReLU(0.2, inplace=True),
            
            nn.Conv2d(in_channels=dim_G_last2_channel*2,
                      out_channels=dim_G_last2_channel*4,
                      kernel_size=4,
                      stride=2,
                      padding=1,
                      bias=False),
            nn.BatchNorm2d(dim_G_last2_channel*4),
            nn.LeakyReLU(0.2, inplace=True),
            
            nn.Conv2d(in_channels=dim_G_last2_channel*4,
                      out_channels=dim_G_last2_channel*8,
                      kernel_size=4,
                      stride=2,
                      padding=1,
                      bias=False),
            nn.BatchNorm2d(dim_G_last2_channel*8),
            nn.LeakyReLU(0.2, inplace=True),
            
            nn.Conv2d(in_channels=dim_G_last2_channel*8,
                      out_channels=1,
                      kernel_size=4,
                      stride=1,
                      padding=0,
                      bias=False),
            nn.Sigmoid()
        )
        
    
    def forward(self, img):
        check_validity = self.model(img)
        
        return check_validity
```

### 6. Train model \\(G\\) and \\(D\\)
#### 6.1 Initialize model \\(G\\) and \\(D\\)

Generative model과 Discriminative model을 선언하고 weight를 초기화해줍니다.
```python
generator = Generator().to(device)
discriminator = Discriminator().to(device)

generator.apply(initialize_weights)
discriminator.apply(initialize_weights);
```

#### 6.2 Loss functions & Optimizers

하이퍼파라미터 `learning_rate`를 정의합니다.
- `learning_rate`: optimizer에 사용되는 learning rate입니다.
- `beta1`: Adam optimizer에 사용되는 momentum parameter입니다.

```python
# hyper-parameter
learning_rate = 0.0002
beta1 = 0.5
```

Loss function은 Binary Cross Entropy (BCE)를 사용합니다. PyTorch 내장함수인 ``torch.nn.BCELoss()`를 이용합니다.
```python
adversarial_loss = nn.BCELoss()
```

optimizer는 각 모델별로 정의합니다. Adam optimizer를 사용합니다.
```python
optimizer_G = optim.Adam(generator.parameters(), lr=learning_rate, betas=(beta1, 0.999))
optimizer_D = optim.Adam(discriminator.parameters(), lr=learning_rate, betas=(beta1, 0.999))
```

#### 6.3 Train models

하이퍼파라미터 `num_epochs`와 `interval_save_img`를 정의합니다.
- `num_epochs`: 최대 Epoch 수를 의미합니다.
- `interval_save_img`: 이미지 생성 결과를 저장할 인터벌을 정의합니다. 일정 batch size마다 이미지가 저장되어 생성모델의 학습과정을 확인할 수 있습니다.

```python
# hyper-parameters
num_epochs = 200
interval_save_img = 1000
```

FloatTensor모델을 정의합니다. 연산 시 변수들의 데이터 타입을 맞춰주기 위함입니다.
```python
Tensor = torch.cuda.FloatTensor if is_cuda else torch.FloatTensor
```

\\(G\\), \\(D\\) 모델에 `num_epochs`만큼 데이터셋을 학습시킵니다. `batch_size`개씩 입력으로 들어갑니다.

전체적인 순서는 아래와 같습니다.

1. 진짜 이미지 정의
2. 가짜 이미지 생성
3. 가짜 이미지와 real값 사이의 loss 계산 (`loss_G`)
4. `loss_G`로 Generator \\(G\\) weight 업데이트
5. 진짜 이미지와 fake값 사이의 loss 계산 (`loss_real`)
6. 가짜 이미지와 real값 사이의 loss 계산 (`loss_fake`)
7. `loss_D` 계산 (`loss_real`과 `loss_fake`의 평균값)
8. `loss_D`로 Discriminator \\(D\\) weight 업데이트
9. 학습상태 print
10. `interval_save_img`에 따라 생성 이미지 저장

```python
losses = []

for idx_epoch in range(num_epochs):
    for idx_batch, (imgs, _) in enumerate(train_data_loader):
        # Ground truth variables indicating real/fake
        real_ground_truth = Variable(Tensor(imgs.size(0), 1, 1, 1).fill_(1.0), requires_grad=False)
        fake_ground_truth = Variable(Tensor(imgs.size(0), 1, 1, 1).fill_(0.0), requires_grad=False) 
        
        # Real image
        real_imgs = Variable(imgs.type(Tensor))
                
        #####################
        # Train Generator
        
        optimizer_G.zero_grad()
        
        # Random sample noise
        z = random_sample_z_space(imgs.size(0))

        # Generate image
        gen_imgs = generator(z)
        
        # Generator's loss: loss between D(G(z)) and real ground truth
        loss_G = adversarial_loss(discriminator(gen_imgs), real_ground_truth)
        
        loss_G.backward()
        optimizer_G.step()
        
        
        #####################
        # Train Discriminator
        
        optimizer_D.zero_grad()
        
        loss_real = adversarial_loss(discriminator(real_imgs), real_ground_truth)
        loss_fake = adversarial_loss(discriminator(gen_imgs.detach()), fake_ground_truth)
        loss_D = (loss_real+loss_fake)/2
        
        loss_D.backward()
        optimizer_D.step()
        
        
        #####################
        # archieve loss
        losses.append([loss_G.item(), loss_D.item()])
        
        # Print progress
        if idx_batch % 10 == 0:
            print("[Epoch {}/{}] [Batch {}/{}] loss_G: {:.6f}, loss_D: {:.6f}".format(idx_epoch, num_epochs,
                                                                                      idx_batch, len(train_data_loader),
                                                                                      loss_G, loss_D))
                    
        batches_done = idx_epoch * len(train_data_loader) + idx_batch
        if batches_done % interval_save_img == 0:
            utils.save_image(gen_imgs.data[:25], "../result/GAN/2-DCGAN/3-{}.png".format(batches_done), nrow=5, normalize=True)
```

> 0 / 1000 / 10000 / 20000 / 30000 / 50000 / 100000번째 batch 학습 결과입니다 (`batch_size=64`). Generative model이 뒤늦게 학습되다가 다시 학습이 불안정해지는 과정을 반복합니다.
> ![GAN G(z) result]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-19-DCGAN-implementation/2020-10-19-DCGAN-implementation-3-0.png)  
> ![GAN G(z) result]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-19-DCGAN-implementation/2020-10-19-DCGAN-implementation-3-1000.png)  
> ![GAN G(z) result]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-19-DCGAN-implementation/2020-10-19-DCGAN-implementation-3-10000.png)  
> ![GAN G(z) result]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-19-DCGAN-implementation/2020-10-19-DCGAN-implementation-3-20000.png)  
> ![GAN G(z) result]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-19-DCGAN-implementation/2020-10-19-DCGAN-implementation-3-30000.png)  
> ![GAN G(z) result]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-19-DCGAN-implementation/2020-10-19-DCGAN-implementation-3-50000.png)  
> ![GAN G(z) result]({{ site.url }}{{ site.baseurl }}/assets/images/post/DL/2020-10-19-DCGAN-implementation/2020-10-19-DCGAN-implementation-3-100000.png)  

200 epoch에 대한 이미지 생성 결과를 시각화해봅니다.
```python

# Random sample noise
z = random_sample_z_space(batch_size)

# Generate image
gen_imgs = generator(z)

# visualize
utils.save_image(gen_imgs.data[:25].cpu().detach(), "../result/GAN/2-DCGAN/4-fake-images.png", nrow=5, normalize=True)
```
> 불안정한 결과로 인해 모델 학습을 중단했습니다

#### 6.4 Save model weights

학습된 가중치를 저장해둡니다.
```python
torch.save({
    'epoch': num_epochs,
    'model_G_state_dict': generator.state_dict(),
    'model_D_state_dict': discriminator.state_dict(),
    'optimizer_G_state_dict': optimizer_G.state_dict(),
    'optimizer_D_state_dict': optimizer_D.state_dict(),
    'losses': losses
}, '../result/GAN/2-DCGAN/model_weights.pth')
```

### 7. Visualization (Interpolation)

GAN과 마찬가지로, Interpolation 그림을 그려봅시다.

학습이 완전하지는 않기 때문에, 사이드 그림이 깔끔한 것을 먼저 골라냅시다.
```python
z_opposites = random_sample_z_space(2)
fake_img = generator(z_opposites)

plt.subplot(121); tc_imshow(img=fake_img[0].cpu().detach() /2+0.5  )
plt.subplot(122); tc_imshow(img=fake_img[1].cpu().detach() /2+0.5  )

plt.savefig('../result/GAN/2-DCGAN/5-side-images.png', dpi=300)
```
> 추가 예정

두 이미지를 사이드 이미지로 두고, 그 사이의 latent space를 interpolation해봅니다.
```python

z_interpolation = Variable(Tensor(np.linspace(z_opposites[0].cpu(), z_opposites[1].cpu(), num_interpolation)))
fake_img = generator(z_interpolation)

plt.figure(figsize=(12,2))
for i in range(num_interpolation):
    plt.subplot(1,num_interpolation,i+1)
    tc_imshow(img=fake_img[i].cpu().detach() /2+0.5 )
    
plt.savefig('../result/GAN/2-DCGAN/6-interpolation.png', dpi=300)
```
> 추가 예정

---

이번 포스팅에서는 DCGAN 구조를 PyTorch로 구현해보았습니다. 하지만 아직 학습이 제대로 이루어졌지 않기 때문에 보완이 필요합니다.

## 이해 못한 부분들 (답을 아시는 분들은 답변 부탁드립니다)

1. 왜 BatchNorm layer들은 평균 0.0이 아닌 1.0으로 초기화 시켰을까요?
2. ConvTranspose layer 들의 `kernel_size`, `padding` 옵션은 어떻게 정해진 건지가 궁금합니다. `stride` 같은 경우에는 그림에 표시되어있어서 확인할 수 있는데, `kernel_size`나 `padding` 옵션은 정보를 찾을 수가 없었습니다.