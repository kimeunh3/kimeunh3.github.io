---
title: '[Python] AWS S3에서 이미지 로드하기'
excerpt_separator: <!--more-->
categories:
  - 'Python'
tags:
  - 'python'
  - 'aws s3'
use_math: true
---

## 모듈 설치

import 해야 되는 모듈은 다음과 같다.

```python
import os
import dotenv
import boto3
import PIL.Image as image
```

각각 모듈은 다음과 같은 명령어로 설치 가능하다.

```
$ pip install python-dotenv
$ pip install boto3
$ pip install pillow
```

os는 python 설치 시 포함되어 있기 때문에 따로 설치가 필요없다!
dotenv는 AWS의 개인적인 정보를 따로 환경변수로 담아두고 불러와 쓰기 위해 사용된다. access key id나 secret access key와 같이 외부에 알려지게 되면 곤란한 정보들은 .env에 모아놓고 .gitignore에 .env를 추가하고 repo에 올리지 않도록 주의하자!

## 코드

```python
# .env 파일 로드
dotenv.load_dotenv()

# AWS S3 setting
AWS_ACCESS_KEY_ID = os.getenv('AWS_ACCESS_KEY_ID')
AWS_SECRET_ACCESS_KEY = os.getenv('AWS_SECRET_ACCESS_KEY')
AWS_REGION_NAME = os.getenv('AWS_REGION_NAME')
AWS_BUCKET_NAME = os.getenv('AWS_BUCKET_NAME')
s3 = boto3.resource('s3',
                    aws_access_key_id=AWS_ACCESS_KEY_ID,
                    aws_secret_access_key= AWS_SECRET_ACCESS_KEY,
                    region_name=AWS_REGION_NAME)
BUCKET = s3.Bucket(AWS_BUCKET_NAME)

def s3_load_image(img_url):
  return BUCKET.Object(img_url).get()['Body']

# S3에 저장된 이미지 파일명이 img.png일 때
img = s3_load_image('img.png')
img = image.open(img)
img.show()

```

대략적인 .env 파일은 아래와 같다!

```
AWS_BUCKET_NAME = ~~
AWS_REGION_NAME = ~~
AWS_ACCESS_KEY_ID = ~~
AWS_SECRET_ACCESS_KEY = ~~
```
