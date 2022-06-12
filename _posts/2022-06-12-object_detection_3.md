---
title: '[딥러닝] 객체 검출(Object Detection) Fast R-CNN'
excerpt_separator: <!--more-->
categories:
  - 'Deep Learning'
tags:
  - 'deep learning'
  - 'object detection'
  - 'fast r-cnn'
use_math: true
---

## 출처

- 이 포스팅은 아래의 강좌를 진행하며 정리한 글입니다.
  > [객체 검출(Object Detection) 딥러닝 기술: R-CNN, Fast R-CNN, Faster R-CNN 발전 과정 핵심 요약](https://youtu.be/jqNCdjOB15s)

## Fast R-CNN

![image](https://user-images.githubusercontent.com/59808674/173233879-986860c9-bd6a-45bd-a328-935477b78e8f.png)

- R-CNN과 비슷하나 feature map을 추출하기 위해 CNN과정을 한 번만 거치게 됨.
- 이후에 RoI(Region of Interest) pooling을 통해 각각의 region들에 대해서 feature에 대한 정보들을 추출 한 뒤 FC(fully connected layer)를 거침.
- CNN을 통해 얻게 된 Feature Map은 input image에 대해서 각각의 위치에 대한 정보를 어느정도 보존하고 있기 때문에 이러한 작업이 가능하게 됨
- 기본적인 CNN 네트워크를 이용해서 Softmax layer를 거쳐 각각의 클래스에 대한 probability를 구함

### RoI Pooling Layer

![image](https://user-images.githubusercontent.com/59808674/173233994-20871c09-50fc-4f4e-b68c-d96c7e7af4cb.png)

- 각 RoI 영역에 대하여 맥스 풀링을 이용해 고정된 크기의 벡터(vector)를 생성
  - **맥스 풀링(max pooling):** 위와 같이 2 x 2 크기의 feature를 추출한다고 하면, 특정 크기의 RoI feature map(빨간색 박스)이 2 x 2로 전체 activation 영역이 최대한 같은 비율로 적절히 나누어질 수 있도록 함. 그 후 나누어진 영역에서 가장 큰 값을 선택

### Pros & Cons

- Feature Extraction, RoI Pooing, Region Classification, Bounding Box Regression 단계를 모두 End-to-End로 묶어서 학습 가능
- 여전히 첫 번째 Selective Search는 CPU에서 수행되므로 속도가 느림
