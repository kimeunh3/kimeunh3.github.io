---
title: '[딥러닝] 객체 검출(Object Detection) Faster R-CNN'
excerpt_separator: <!--more-->
categories:
  - 'Deep Learning'
tags:
  - 'deep learning'
  - 'object detection'
  - 'faster r-cnn'
use_math: true
---

## 출처

- 이 포스팅은 아래의 강좌를 진행하며 정리한 글입니다.
  > [객체 검출(Object Detection) 딥러닝 기술: R-CNN, Fast R-CNN, Faster R-CNN 발전 과정 핵심 요약](https://youtu.be/jqNCdjOB15s)

## Faster R-CNN

![image](https://user-images.githubusercontent.com/59808674/173355627-b8f275c3-9973-4fc6-9139-4e155127c1d2.png)

- R-CNN과 Fast R-CNN 모두 CPU에서 Region proposal을 진행하기 때문에 속도가 느림. 그러한 점을 개선하기 위해 Region proposal을 위한 모든 양산을 gpu상에서 수행할 수 있도록 하는 RPN 네트워크를 지원
- 과정은 Fast R-CNN에서 Region proposal을 진행하는 Selective Search를 RPN으로 바꾸어 모든 과정 자체를 end-to-end방식으로 학습 될 수 있도록 architecture를 바꾼 구조
  - End-to-End(종단간): 입력에서 출력까지 '파이프라인 네트워크' 없이 한 번에 처리

### RPN(Region Proposal Network)

![image](https://user-images.githubusercontent.com/59808674/173356139-74caa7ed-b3b1-48b9-a372-a195e467ff58.png)

- 딥러닝 모델이기 때문에 GPU로 올려 모델을 돌릴 수 있음
- Feature map을 보고 어느 곳에 물체가 있을 법한 지 예측할 수 있도록 하여 Selective Search의 시간적인 단점을 해결하는 대안
  - 다양한 형태와 크기를 가지는 물체를 잘 예측할 수 있도록 k개의 앵커 박스(anchor box)를 이용
  - 왼쪽 위부터 오른쪽 아래까지 슬라이딩 윈도우(sliding window)를 거쳐 각 위치에 대해 intermediate feature를 뽑고 Regression과 Classification을 수행
    - Classification: Class에 대한 결과가 아닌 물체가 있는 지에 대한 여부만 두 개의 Output(0 or 1)으로 나타냄
    - Regression: 물체가 존재하는 위치를 정확히 찾기 위해 bounding box의 중간점인 x, y 좌표와 너비와 높이 값을 예측
- 학습이 이뤄지고 난 뒤에는 gpu상에서 한번의 forwarding만 하면 어느 곳에 물체가 있을 법 한지 예측 가능
