---
title: '[Python] venv 가상환경 구축하기'
excerpt_separator: <!--more-->
categories:
  - 'Python'
tags:
  - 'python'
  - 'venv'
use_math: true
---

## 가상환경 구축  

```
$ python -m venv 가상환경이름
```

가상환경 구축으로 인해 가상환경이름으로 폴더가 생기는데 그 폴더는 레포에 올릴 필요가 없으니 .gitignore에 추가해주는 것이 좋다.


## 가상환경 활성화

Windows
```
$ source venv/Scripts/activate
```
Mac
```
$ source venv/bin/activate
```
$ 앞이나 프롬프터 앞에 \(가상환경이름)가 뜬다면 활성화 완료!  

git bash에선 다음과 같이 뒤에 뜬다.. 왜인지는 아직 의문..  
![image](https://user-images.githubusercontent.com/59808674/175033501-6ed6a43d-c065-4bdc-ab24-10af86a96372.png)  

활성화 후에는 프로젝트당 필요한 모듈들을 설치해준다. requirements.txt가 있다면 다음과 같이 설치할 수 있다.  

```
$ pip install -r requirements.txt
```

추가적인 모듈설치가 있었다면 requirements.txt를 만드는 방법은 다음과 같다.  

```
$ pip freeze > requirements.txt
```


## 가상환경 비활성화  

```
$ deactivate
```