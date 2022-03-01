---
title: "[딥러닝] Intro to TensorFlow"
excerpt_separator: <!--more-->
categories: 
    - "Deep Learning"
tags: 
    - "machine learing"
    - "deep learning"
    - "tensorflow"
use_math: true
---
## 프로젝트 소개  
 - 이 프로젝트는 아래 링크의 생활코딩의 딥러닝 강좌를 진행하며 학습하였습니다.  
 > [Tensorflow (python)](https://opentutorials.org/course/4570/28965)  

### 프로젝트 동기  
 - 생활코딩의 [머신러닝1](https://opentutorials.org/course/4548) 강좌를 들은 뒤 딥러닝에도 흥미가 생겼고 실제로 코드를 작성하며 실습하고 결과를 보며 배워 나가고 싶어 진행하게 되었습니다.

### 프로젝트 언어 및 환경  
 - 프로그래밍 환경: Google Colabolatory  
 - 프로그래밍 언어: Python  
 - 딥러닝 라이브러리: TensorFlow  

## 1. 오리엔테이션

### 머신러닝 알고리즘
 - 이 강좌에서 다룰 지도학습(supervised learning)의 분류(classification)와 회귀(regression)와 같은 문제들을 풀기 위한 방법입니다.  
    - 대표적으로 Decision Tree, Random Forest, KNN, SVM, **Neural Network**가 있습니다.  

### Neural Network = 인공신경망 = Deep Learning
 - 사람의 두뇌가 동작하는 방법을 모방하여 기계가 학습을 할 수 있도록 고안된 알고리즘입니다.  
 - **뉴런(Neuron)** 이라는 세포들로 촘촘하게 연결되어 있는 사람의 뇌처럼 뉴런들로 연결되어 있는 신경망을 인공적으로 만들었다는 의미에서 **인공신경망**이라고도 합니다.  
 - 인공신경망을 깊게 쌓아서 만들었다는 표현으로 **Deep Learning**이라는 단어를 사용하기 시작했습니다.  
 - 결과적으로 Neural Network, 인공신경망, Deep Learning은 모두 **인간의 신경을 모방한 이론**을 가르키는 같거나 비슷한 말입니다.  

### 라이브러리
 - 구체적인 딥러닝의 원리를 몰라도 코드만 작성하면 딥러닝으로 문제를 해결할 수 있는 도구들입니다.
 - 대표적으로 **TensorFlow**, PyTorch, Caffe, Theano가 있습니다.
## 2. 목표와 전략  

### 목표
 - 딥러닝 코드들의 사용법을 알고 각자의 데이터로 놀아볼 수 있게 되는 것입니다.  

### 공부 전략 (aka 지도 학습법)  
 1. 원인이 되는 간단한 코드를 작성하고 경험합니다.  
 2. 결과를 보며 코드의 동작과 학습과정을 확인합니다.  
 3. 해당 코드를 어떻게 이용하면 좋을지 추측합니다.  
 4. 위의 과정을 반복하며 코드와 알고리즘 동작에 익숙해집니다.  

## 3. 지도학습의 빅픽쳐  

### 지도학습의 과정  
 1. 과거의 데이터를 준비합니다.  
 2. 모델의 구조를 만듭니다.  
 3. 데이터로 모델을 학습(FIT)합니다.  
 4. 모델을 이용합니다.  

## 4. 표를 다루는 도구 판다스  
 - [실습 코드](https://github.com/kimeunh3/Intro_to_Tensorflow/blob/main/%EC%8B%A4%EC%8A%B51_%ED%91%9C%EB%A5%BC_%EB%8B%A4%EB%A3%A8%EB%8A%94_%EB%8F%84%EA%B5%AC_%ED%8C%90%EB%8B%A4%EC%8A%A4.ipynb)  

### 실습을 통해 배울 도구들  
 - 파일 읽어오기: `pd.read_csv('/경로/파일명.csv')`  
 - 모양 확인하기: `print(데이터.shape)`  
 - 칼럼 선택하기: `데이터[['칼럼명1', '칼럼명2', '칼럼명3']]`  
 - 칼럼 이름 출력하기: `print(데이터.columns)`  
 - 맨 위 5개 관측치 출력하기: `데이터.head()`  

## 5. 첫 번째 딥러닝 - 레모네이드 판매 예측  
 - [실습 코드](https://github.com/kimeunh3/Intro_to_Tensorflow/blob/main/%EC%8B%A4%EC%8A%B52_%EB%A0%88%EB%AA%A8%EB%84%A4%EC%9D%B4%EB%93%9C_%ED%8C%90%EB%A7%A4_%EC%98%88%EC%B8%A1.ipynb)  

### 손실(Loss)의 의미
 - 각 epoch(학습의 횟수)마다 그 시점에 모델이 얼마나 정답에 가까이 맞추고 있는지 평가하는 지표입니다.
 - 예측과 결과를 비교하여 구한 차이의 제곱값들의 평균입니다.
 - loss가 0에 가까워질수록 학습이 잘 된 모델이 되었다고 할 수 있습니다.
 - epoch마다 loss가 0에 가까워지고 있는지 확인하며 loss가 원하는 수준으로 떨어질 때까지 반복해서 학습을 시키면 됩니다.  

## 6. 두 번째 딥러닝 - 보스턴 집값 예측  
 - [실습 코드](https://github.com/kimeunh3/Intro_to_Tensorflow/blob/main/%EC%8B%A4%EC%8A%B53_%EB%B3%B4%EC%8A%A4%ED%84%B4_%EC%A7%91%EA%B0%92_%EC%98%88%EC%B8%A1.ipynb)  

### 중앙값 (median value)  
 - 주어진 값을 순서대로 정렬한 다음에 그 중에서 가장 가운데에 있는 값이 **중앙값(median value)** 입니다.  
 - 중앙값과 함께 집단을 대표하기 위해 사용하는 값으로 **평균값(average value)** 이 있습니다.
    - 일반적으로 평균값이 집단을 대표하기 위해 좋은 지표이지만 너무 높은 값이나 너무 낮은 값이 평균값을 취약하게 만들어버리는 경우가 있습니다.
    - 이렇게 전체 집단의 수치와 비교하여 상이하게 높거나 낮아서 평균의 대표성을 무너뜨리는 값들을 **이상치(outlier)** 라고 합니다. 
 - 이상치로 인해 평균이 대표성을 띠지 못하는 경우에 그 대안으로 사용하는 값이 중앙값입니다.  

### 퍼셉트론 (Perceptron)  
 - `y = w1x1 + w2x2 + ... + w13x13 + b`
 - 인공신경망에서 두뇌 세포 뉴런의 역할을 하는 것이 **퍼셉트론(Perceptron)** 입니다.
 - 위의 식에서 각 w는 **가중치(weight)** 를 뜻하고, b는 **편향(bias)** 을 뜻합니다.

## 7. 세 번째 딥러닝 - 아이리스 품종 분류  
 - [실습 코드](https://github.com/kimeunh3/Intro_to_Tensorflow/blob/main/%EC%8B%A4%EC%8A%B54_%EC%95%84%EC%9D%B4%EB%A6%AC%EC%8A%A4_%ED%92%88%EC%A2%85_%EB%B6%84%EB%A5%98.ipynb)  

### 원핫인코딩 (onehot-encoding)  
 - 범주형 데이터를 1과 0의 데이터로 바꿔주는 과정을 **원핫인코딩(onehot-encoding)** 이라고 합니다.  
 - 딥러닝 모델을 사용하기 위해서는 모든 범주형 변수를 원핫인코딩 해줘야 합니다.  

### 소프트맥스 (Softmax)  
 - 분류 예측을 비율로 예측하기 위해 **소프트맥스(Softmax)** 를 사용합니다.  
 - 소프트맥스 함수는 값을 0과 1사이의 값으로 변환해줍니다.  
 - 회귀모델에서는 Identity 함수가, Softmax 함수는 분류모델에서 사용됩니다.  
    - Identity 함수는 값을 바꾸지 않은 형태 그대로를 내보내는 함수입니다.  
 - 이렇게 y의 값의 형태를 바꿔주는 모든 함수들을 **활성화(activation) 함수** 라고 부릅니다.  

### Cross Entropy  
 - 분류모델에 사용하는 loss입니다.  
 - 회귀모델에는 **MSE(Mean Squared Error)** 를 사용합니다.  
 - 분류모델에서는 사람이 보기에 더 좋은 평가 지표로 **정확도(accuracy)** 를 사용하기도 합니다.  
    - 1은 정확도 100%를, 0은 정확도 0%를 뜻합니다.  

## 8. 히든레이어  
 - [실습 코드](https://github.com/kimeunh3/Intro_to_Tensorflow/blob/main/%EC%8B%A4%EC%8A%B55_%ED%9E%88%EB%93%A0%EB%A0%88%EC%9D%B4%EC%96%B4.ipynb)  

### 히든레이어 (hidden layer)  
 - 입력 부분(input layer)과 출력 부분(output layer) 사이에 추가한 부분을 **히든레이어(hidden layer)** 라고 부릅니다.  
 - 입력 부분과 히든레이어를 연결하는 모델, 히든레이어와 출력부분을 연결하는 모델, 이렇게 각각의 모델을 연속적으로 연결하여 하나의 거대한 신경망을 만드는 것이 **딥러닝, 인공신경망** 입니다.  

## 부록1. 데이터를 위한 팁
 - [실습 코드](https://github.com/kimeunh3/Intro_to_Tensorflow/blob/main/%EC%8B%A4%EC%8A%B56_%EB%8D%B0%EC%9D%B4%ED%84%B0_%ED%83%80%EC%9E%85_%EC%A1%B0%EC%A0%95.ipynb)  

### 데이터 타입 조정
 - 변수(칼럼) 타입 확인
    - `데이터.dtypes`
 - 변수를 범주형으로 변경
    - `데이터['칼럼명'].astype('category')`
 - 변수를 수치형으로 변경
    - `데이터['칼럼명'].astype('int')`
    - `데이터['칼럼명'].astype('float')`
 - NA 값의 처리
    - NA 갯수 체크
        - `데이터.isna().sum()`  
    - NA 값 채우기
        - `데이터['칼럼명'].fillna(특정숫자)`

## 부록2. 모델을 위한 팁
 - [실습 코드](https://github.com/kimeunh3/Intro_to_Tensorflow/blob/main/%EC%8B%A4%EC%8A%B57_%ED%95%99%EC%8A%B5%EC%9D%B4_%EC%9E%98%EB%90%98%EB%8A%94_%EB%AA%A8%EB%8D%B8.ipynb)  
