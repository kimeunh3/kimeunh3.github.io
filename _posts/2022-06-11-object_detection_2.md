---
title: '[딥러닝] 객체 검출(Object Detection) R-CNN'
excerpt_separator: <!--more-->
categories:
  - 'Deep Learning'
tags:
  - 'deep learning'
  - 'object detection'
  - 'r-cnn'
use_math: true
---

## 출처

- 이 포스팅은 아래의 강좌를 진행하며 정리한 글입니다.
  > [객체 검출(Object Detection) 딥러닝 기술: R-CNN, Fast R-CNN, Faster R-CNN 발전 과정 핵심 요약](https://youtu.be/jqNCdjOB15s)

## R-CNN

<img width="840" alt="image" src="https://user-images.githubusercontent.com/59808674/173185820-6818ba3d-b04a-4109-a952-8f8ea30c803f.png">

### 동작과정

1. **Selective Search:** 물체가 존재할 법한 위치, 약 2000개의 RoI(Region of Interest)를 추출
2. **Crop & Resize:** 각 RoI에 대하여 warping을 수행; 동일한 크기의 입력 이미지로 변경
3. **CNN:** Crop & Resize 과정을 거친 이미지를 CNN에 넣고(forward) 이미지 feature vector를 추출
4. **SVMs and Regressors:** SVM으로 Classification을, Regressor로 정확한 물체의 위치의 bounding box를 조절하여 예측
    - 여러 개의 클래스가 존재하는 상황에서 각각의 클래스에 대해 독립적으로 훈련된 이진(binary) SVM을 사용  

### R-CNN의 한계점

- 입력 이미지에 대해 CPU 기반의 Selective Search를 진행하여 속도가 느림
- 전체 architecture에서 SVM, Regressor 모듈이 CNN과 분리되어 있음;
  - CNN이 고정 되어 있어 SVM과 Bounding Box Regression 결과로 CNN를 업데이트 불가능
  - end-to-end 방식 학습이 불가능
- 모든 RoI를 CNN에 넣어야해서 2,000번의 CNN 연산이 필요
  - 학습(Training)과 평가(Testing) 모두 많은 시간 소요