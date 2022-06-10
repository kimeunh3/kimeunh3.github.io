---
title: '[딥러닝] 객체 검출(Object Detection) 딥러닝 기술'
excerpt_separator: <!--more-->
categories:
  - 'Python for Coding Test'
tags:
  - 'deep learning'
  - 'object detection'
use_math: true
---

## 출처

- 이 포스팅은 아래의 강좌를 진행하며 정리한 글입니다.
  > [객체 검출(Object Detection) 딥러닝 기술: R-CNN, Fast R-CNN, Faster R-CNN 발전 과정 핵심 요약](https://youtu.be/jqNCdjOB15s)

## 사물을 인식하는 다양한 문제 상황

![image](https://user-images.githubusercontent.com/59808674/173055532-ec517b8d-8555-401f-89fd-8c58aff3ddd4.png)

- **Classification:** 하나의 이미지를 입력받아 해당 이미지가 어떤 클래스 인지 맞히는 문제
- **Localization:** 지역화, 국소화; 위치를 찾는 문제; 이미지 내에 존재하는 하나의 물체가 어디에 존재하는지 bounding box로 표시하는 것
- **Object Detection:** 다수의 사물이 존재하는 이미지에서 각 사물의 위치와 클래스를 찾는 작업; 각각의 bounding box로 위치 표시
- **Instance Segmentation:** 각각의 사물 instance를 pixel단위로 구분하는 것

## 객체 검출 방식

### 2-Stage Detector

![image](https://user-images.githubusercontent.com/59808674/173055926-33daca27-6be7-49b9-8711-4ee74c1f1ec0.png)

- 물체의 위치를 찾는 **문제(localization)** 와 **분류(classification)** 문제를 **순차적으로** 해결
- ex) R-CNN, Fast R-CNN, Faster R-CNN

### 1-Stage Detector

![image](https://user-images.githubusercontent.com/59808674/173056006-eeca1bcc-80ae-4767-ba24-2cfde612aca8.png)

- 물체의 위치를 찾는 문제(localization)와 분류(classification)문제를 **한 번에** 해결
- 일반적으로 2-Stage Detector보다는 동작속도가 빠르지만 정확도가 낮음
- ex) YOLO

## Region Proposal

- 물체가 있을 법한 위치 찾기

### Sliding Window

![image](https://user-images.githubusercontent.com/59808674/173056495-69d975a0-8734-4548-a8e1-b0b2602df148.png)

- 이미지에서 다양한 형태의 **윈도우(window)** 를 **슬라이딩(Sliding)** 하며 물체가 존재하는 지 확인
- 너무 많은 영역에 대하여 확인해야하는 단점(속도 저하)
- Faster R-CNN은 GPU에서 Sliding Window 방식을 사용

### Selective Search

![image](https://user-images.githubusercontent.com/59808674/173056663-8e0df5b8-fc29-4c6b-bc75-89a36f43059b.png)

- 인접한 영역(region)끼리 유사성을 측정해 큰 영역으로 차례대로 통합해 나감
- R-CNN, Fast R-CNN에서 쓰임
- CPU에서 수행되도록 library가 작성되어 있어 이미지당 2초 가량이 소요될 수 있음

## 정확도(Precision)와 재현율(Recall)

![image](https://user-images.githubusercontent.com/59808674/173056832-5fd0dc62-5d55-4880-ad4b-07282a1cd9f8.png)

- **정확도(Precision)**
  - 올바르게 탐지한 물체의 수(TP) / 모델이 탐지한 물체의 개수(TP + FP)
- **재현율(Recall)**
  - 올바르게 탐지한 물체의 수(TP) / 실제 정답 물체의 수(TP + FN)
- 모든 영역에 대하여 전부 사물이 존재한다고 판단할 경우
  - 재현율은 높아지지만 정확도는 감소
- 매우 확실할 때만 (confidence가 높을 때만) 사물이 존재한다고 판단할 경우
  - 정확도는 높아지지만 재현율은 감소

## Average Precision

- 일반적으로 정확도와 재현율은 반비례 관계를 가짐
- 정확도와 재현율을 모두 고려하는 Average Precision으로 성능 평가하는 경우가 많음
- 모델의 예측 Confidence에 따른 단조 감소 그래프로 넓이를 계산

![image](https://user-images.githubusercontent.com/59808674/173058532-370bbba9-3155-4f39-8834-b5bcfd859c4d.png)

## Intersection over Union (IoU)

![image](https://user-images.githubusercontent.com/59808674/173058614-1e4581b5-0807-492b-a2d1-8d7697a57600.png)

- 예측한 바운딩 박스가 실제 정답 바운딩 박스와 얼마나 유사한지 평가하는 척도
- 두 바운딩 박스가 겹치는 비율: Area of Overlap / Area of Union
  - 성능 평가 예시: mAP@0.5는 정답과 예측의 IoU가 50% 이상일 때 정답으로 판정하겠다는 의미
  - NMS 계산 예시: 같은 클래스끼리 IoU가 50% 이상일 때 낮은 confidence의 box를 제거
    - **NMS(non maximum suppresion):** 하나의 인스턴스(instance)에 하나의 bounding box가 적용되어야 하므로 여러 개의 bounding box가 겹쳐있는 경우에 하나로 합치는 방법
