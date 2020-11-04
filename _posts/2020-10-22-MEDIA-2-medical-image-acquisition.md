---
title: "Medical Image acquisition methods"
excerpt: "X-ray, CT, PET, MRI와 같은 의료영상 측정방법의 원리를 정리해봅시다"

categories:
- Medical image analysis

tags:
- Medical image analysis
- Lecture
- Classification

toc: true
toc_sticky: true
toc_label: "Medical Image acquisition methods"

use_math: true
---

이전 포스팅: [Introduction to MEDIA]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/MEDIA-1-introduction-to-MEDIA/)

> 이전 포스팅에서는 Medical Image Analysis 시리즈에서 다룰 내용을 정리했습니다.  
> 이번 포스팅에서는 Medical Image 측정에 사용되는 여러 장비들의 원리를 간단히 정리해봅니다.

## PACS

의료영상 측정 장비 설명에 앞서, 먼저 PACS를 소개해봅시다.

Picture Archiving and Communication System (PACS)는 병원에서 사용하는 의료 정보를 위한 데이터 공유 시스템입니다. 

![2020-10-22-medical-image-acquisition-1-pacs]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-1-pacs.png)

X-ray, CT 등의 측정을 하고 서버에 저장한 뒤 End-user (의사)의 쿼리에 반응하여 데이터를 제공합니다.

## DICOM

이 때 사용되는 데이터 포맷 중 하나가 바로 Digital Imaging and Communications in Medicine (DICOM)입니다. DICOM은 북미 방사선 학회 (Radiological Society of North America, RSNA)에서 의료영상에 대한 공식 표준으로 정한 데이터 포맷입니다.

DICOM 이미지 (`*.dcm`) 안에는 의료영상뿐 아니라 몇 가지 간단한 의료 기록들 및 영상에 대한 헤더 정보 (dimension, voxel spaciong, origin 등)도 함께 저장됩니다.

DICOM은 헤더와 영상 정보가 한 파일에 합쳐진 데이터 포맷입니다. 다른 의료 영상 데이터 포맷들은 `*.hdr`/`*.img` (헤더/영상), `*.mhd`/`*.raw` (헤더/영상), `*.nii`/`*.nii.gz`(헤더+영상, 2D/3D) 등이 있습니다.

### Visualization

DICOM 이미지를 시각화하는 툴은 여러가지가 있는데, ITK-SNAP, 3D Slicer, ImageJ 등이 사용된다고 합니다. Pathology 연구에는 QuPath도 사용된다고 합니다.

![2020-10-22-medical-image-acquisition-2-itk-snap]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-2-itk-snap.png)

예시를 보면, axial, saggital, coronal view로 3D 이미지를 2D로 볼 수 있으며, 사용자가 여러 장의 이미지에 마킹을 하게 되면 3D로 표현해주는 기능도 있습니다.

## Medical Imaging methods

Medical Image는 다양한 장비로 측정됩니다. 아래와 같이 Endoscopy, Microscopy 부터 X-ray, CT, PET, MRI까지 다양한 측정 장비가 존재합니다.

- Endoscopy (내시경)
- Microscopy
- X-ray
- Computed Tomography (CT)
- Positron Emission Tomography (PET)
- Magnetic Resonance Imaging (MRI)
- Ultrasound
- Optimal Coherence Tomography (OCT)

### Common mechanism

이 장비들의 센싱 방식은 대부분 비슷합니다. 바로 **빛**을 센싱하는 것입니다.

![2020-10-22-medical-image-acquisition-3-light-sensor-mechanism]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-3-light-sensor-mechanism.png)

빛을 쏘면, 물체는 빛을 반사시킵니다. 그리고 CMOS나 CCD와 같은 센서로 이 빛을 전기신호로 변환할 수 있습니다. 전기신호의 값을 이용해 색과 명암 등을 구별합니다.

![ 2020-10-22-medical-image-acquisition-4-light-wavelength ]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-4-light-wavelength.png)

하지만 색, 명암 등에 사용되는 가시광선의 파장대는 매우 적은 비율을 차지합니다. 의료영상에서는 주로 가시광선보다 파장대가 작은 (즉 에너지가 큰) Ultra-violet, X-ray, \\(\gamma\\)-ray 를 사용합니다.

그 이유는 파장대가 큰 Radio waves나 Infrared light의 경우, 인체를 통과하지 못합니다. 따라서 무언가를 센싱할 수 없는 반면, 파장대가 짧은 light들은 인체를 통과하여 그 뒤에서 빛을 센싱할 수 있습니다.

> 📌  
> 한 가지 주의할 점은, 에너지가 크기 때문에 인체에 주는 해로운 영향도 크다는 점입니다 (방사선 노출).

### Endoscopy

![ 2020-10-22-medical-image-acquisition-5-endoscopy]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-5-endoscopy.png)

내시경입니다.

앞의 설명과 마찬가지로 빛을 쏘는 부분과 반사되는 빛을 탐지하는 센서 부분이 있습니다.

### Microscopy

![2020-10-22-medical-image-acquisition-6-microscopy]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-6-microscopy.png)

현미경은 두 개의 렌즈로 이루어져서, 상에 맺히는 물체를 확대시킵니다. 병리학 (pathology) 에서 많이 사용되며, 최근에는 디지털 현미경도 도입되는 추세라고 합니다.

### X-ray

![2020-10-22-medical-image-acquisition-7-x-ray]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-7-x-ray.png)

앞서 설명한 바와 같이 X-ray는 파장이 작아, 인체를 통과합니다.

이 때, 공기를 지나는 X-ray와 뼈를 지나는 X-ray는 투과되는 정도가 다르기 때문에, 인체 뒤에서 detector로 X-ray를 센싱할 때, intensity의 차이가 생기게 됩니다. 이를 전기신호로 변환해준 뒤 이미지화시키면, 결과적으로 뼈 부분은 밝게, 공기 부분은 어둡게 표현된 의료영상을 얻을 수 있습니다.

### CT

![2020-10-22-medical-image-acquisition-8-CT]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-8-CT.png)

CT (Compted Tomography)는 X-ray를 여러 장 쌓아 3D화 시킨 것이라고 생각하면 됩니다.
말 그대로 여러 방향에서 X-ray를 찍은 뒤, 이를 쌓아 입체화시킵니다.

![2020-10-22-medical-image-acquisition-9-CT-result]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-9-CT-result.png)

결과적으로는 이런 3D 이미지를 얻게됩니다.

![2020-10-22-medical-image-acquisition-10-CT-mechanism]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-10-CT-mechanism.png)

예를 들어 1D로 흰 물체를 센싱한다고 할 때, 여러 방향의 센싱값을 합침으로써 원래 이미지를 잘 형상화할 수 있게 됩니다. 하지만, 많은 X-ray에 노출되다보니 인체에 더욱 더 유해하겠죠. 그래서 적은 X-ray 촬영횟수로 높은 퀄리티의 CT영상을 reconstruction 하는 것이 이슈 중 하나라고 합니다.

### PET

X-ray와 CT는 방사선을 인체에 투과시켜 뒤에 있는 detector로 센싱했습니다. Positron Emission Tomography (PET)는 이와 반대로, 인체에 방사선 물질을 주입하고, 인체에서 새어 나오는 방사선을 측정합니다.

![2020-10-22-medical-image-acquisition-11-PET]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-11-PET.png)

주로 반감기가 짧은, 포도당과 유사한 방사선 물질을 투여하는데, 이 물질의 특징은 **포도당 대사 (metabolic function)**가 활발한 곳에서 양전자 (positron)를 방출**합니다. 양전자들은 신체 내 전자와 만나 소멸하면서 \\(\gamma\\)-ray를 발생시키고, \\(\gamma\\)-ray는 체외로 양방향으로 방출됩니다. 이를 모든 외부 방향에서 detect 한 뒤, CT처럼 이미지를 쌓아 3D화시킵니다.

> 📌  
> 암세포가 있는 곳에서 포도당 대사가 비정상적으로 많이 발생하는 경향이 있기 때문에, 주로 암 조기 진단에 활용됩니다.

PET는 결과 이미지를 보다시피, spatial resolution이 그리 좋지는 않습니다.

### MRI

MRI는 앞서 살펴본 X-ray, CT, MRI와 다르게 방사선을 이용하지 않고, 수소 원자의 공명 현상을 이용합니다.

![2020-10-22-medical-image-acquisition-12-MRI-mechanism]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-12-MRI-mechanism.png)

몸 안에는 무수히 많은 수소 원자가 random한 방향으로 놓여 (?)있습니다.

강한 magnetic field (1.5T, 3T, 7T ...)가 가해지면, 수소원자핵들은 자기장에 의해 정렬합니다. 그리고 세차운동을 하며 빙글빙글 돌게 됩니다.

![2020-10-22-medical-image-acquisition-13-MRI-mechanism]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-13-MRI-mechanism.png)

이 떄 세차운동의 속도를 계산하여 적합한 RF pulse를 가해주게 되면, 공명현상이 일어납니다.

![2020-10-22-medical-image-acquisition-14-MRI-mechanism]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-14-MRI-mechanism.png)

즉, 자기장만을 가했을 때는, 정렬된 수소원자들이 다 같은 방향 (z축)을 가리키고 있지만, 공명현상이 함께 일어나게 되면, 일부 수소원자핵은 에너지를 갖게 됩니다. 이에 따라 다른 방향 (y축)의 자기장을 띄게 되고, 이들이 모이게 되면 결과적으로 총 자기장 방향이 바뀌게 됩니다. 주로 눕는다고 표현합니다.

![2020-10-22-medical-image-acquisition-15-MRI-mechanism]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-15-MRI-mechanism.png)

이 때, RF pulse를 끊게 되면, RF pulse에 의해 생겼던 수소원자핵의 에너지가 사라지기 때문에, 누워있던 자기장 방향이 원래대로 돌아옵니다.

![2020-10-22-medical-image-acquisition-16-MRI-mechanism]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-16-MRI-mechanism.png)

이 때 돌아오는 속도를 측정하여, 원래 자기장 방향 (z축)의 신호가 일정 수준 돌아왔을 때 측정된 이미지를 T1-weighted, 누웠던 자기장 방향 (y축)의 신호가 일정 수준으로 떨어졌을 때 측정된 이미지를 T2-weighted 이미지라고 합니다.

MRI는 또한 coil의 신호가 변화하는 속도가 다르기 때문에, tissue간의 contrast가 가능합니다. 이에 따라 soft tissue의 측정에 유리하다고 합니다.

![2020-10-22-medical-image-acquisition-17-MRI-mechanism]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-17-MRI-mechanism.png)

MRI의 작동 원리입니다. RF pulse를 쏘는 Radio Frequency coils과 자기장을 만드는 Magnet을 확인할 수 있습니다. 추가적으로 Gradient coils 라는 게 있습니다. Gradient coils이 없다면 몸의 모든 부분이 같은 값을 나타낼 것입니다. 이렇게되면 원하는 부위의 MRI 영상을 얻을 수 없습니다. 

이는 몸의 각 부분 (머리/가슴/배/다리 등...)마다 다른 자기장을 가해주는 코일입니다.

예를 들어 머리에는 0.1T를, 가슴에는 0.01T를, 다리에는 0.001T를 가해주면, 수소원자핵의 세차운동 속도가 달라집니다. 결과적으로 이미지에 영향을 주기 때문에, 특정 영역만 측정이 가능토록 하는 장치입니다.

### Pros and Cons

지금까지 의료영상 측정에 사용되는 장비들의 원리를 간단히 정리해보았습니다.

![2020-10-22-medical-image-acquisition-18-comparison-CT-PET-MRI]({{ site.url }}{{ site.baseurl }}/assets/images/post/MEDIA/2020-10-22-medical-image-acquisition/2020-10-22-medical-image-acquisition-18-comparison-CT-PET-MRI.png)

마지막으로 CT, PET, MRI의 장단점을 정리하고 마무리합니다.

- CT는 짧은 측정 시간이 장점이며, 상대적으로 저렴합니다. 다만, 방사선에 노출되며 (혈관 등 특정 목적 측정 시) 조영제를 투여하게 되는데, 조영제 배출이 잘 안되는 환자에게는 치명적일 수 있습니다.

- PET는 신진대사활동을 측정할 수 있어, 암세포의 조기진단에 유리합니다. 다만 CT와 마찬가지로 X-ray에 노출되며, 방사선물질을 투여하기 위한 cyclotron 장비가 비쌉니다.

- MRI는 방사선 걱정이 없고, soft tissue의 측정에 용이합니다. 다만 긴 측정 시간과, 자기장에 의한 큰 소음이 발생하며, 가격이 비싼 편입니다.

---

다음 포스팅: [Classification for MEDIA (1)]({{ site.url }}{{ site.baseurl }}/medical%20image%20analysis/MEDIA-3-classification-for-medical-image-1/)