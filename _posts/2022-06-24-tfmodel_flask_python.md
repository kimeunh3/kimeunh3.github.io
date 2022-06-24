---
title: '[Python] Flask로 tensorflow SavedModel 배포하기'
excerpt_separator: <!--more-->
categories:
  - 'Python'
tags:
  - 'python'
  - 'flask'
  - 'tensorflow'
  - 'savedmodel'
use_math: true
---

## 왜 Flask?

Flask란 Python을 사용해서 Web Server를 만들 수 있게 도와주는 Web Framework다!

인공지능 프로젝트에서 인공지능 모델을 웹서비스에 적용하는 과정에서 처음에는 tensorflow.js를 사용했는데, 생각보다 소스코드가 많이 없어서 적용하는 과정에서 시행착오가 많았다..ㅠㅠ 심지어 인공지능 모델이 계산하는 과정이 너무 느려서 웹서비스 상에서 사용하기 어려울 정도였다.  

중간발표 후 많은 팀들이 tensorflow.js가 아닌 Flask를 이용해 인공지능 서버를 구축하여 적용하는 것을 보고 시도해보았고, Python에서 JavaScript로 변환하는 어려움도 겪지 않게되어 생각보다 수월하게 적용가능했다.

## 모듈 설치

Flask 모듈 설치는 다음과 같다!

```
$ pip install Flask
```

## 코드
```python
import tensorflow as tf
import PIL.Image as image
from flask import Flask, jsonify, request

app = Flask(__name__)

# 모델에 맞는 포맷에 이미지를 변환해주는 과정
def transform_images(img, size): 
  img = tf.expand_dims(img, 0)
  img = tf.image.resize(img, (size, size))
  img = img / 255
  return img

@app.route("/", methods=["GET"]) 
def predict():    
  # /?img=파일이름
  imgurl = request.args.get("img")
  outputs="please input image url"
  if imgurl != None:                     
    imgurl=imgurl.split("?")[0]
    # 같은 폴더내에 img폴더가 있고 그 안에 있는 이미지를 불러올 경우
    img = image.open("img/"+imgurl)          
    img = transform_images(img, 416)   
    # 아래 모델을 작동시키는 코드는 모델마다 다를 수 있다!    
    infer = model.signatures[tf.saved_model.DEFAULT_SERVING_SIGNATURE_DEF_KEY]
    outputs = infer(img)   
    # numpy 배열은 json화 할수 없기 때문에 list로 변환해주는 과정
    boxes, scores, classes, nums = outputs["yolo_nms"], outputs["yolo_nms_1"], outputs["yolo_nms_2"], outputs["yolo_nms_3"]
    detected_num = nums.numpy()[0]
    boxes, scores, classes, nums = boxes.numpy()[0][:detected_num].tolist(), \
                                   scores.numpy()[0][:detected_num].tolist(), \
                                   classes.numpy()[0][:detected_num].tolist(), \
                                   int(nums.numpy()[0])
    class_names = [c.strip() for c in open(OD_CLASS_NAME_PATH).readlines()]
    classes = [ class_names[i] for i in classes ]
    outputs = { "nums": nums, "boxes": boxes, "scores": scores, "classes": classes }
  return jsonify(outputs)

if __name__ == "__main__":
  # 같은 폴더 내에 saved_model 폴더가 있을 경우
  model = tf.keras.models.load_model("saved_model") 
  app.run(host="0.0.0.0", port=8080) 
```