---
title: face_recognition图片人脸识别
date: 2019-03-09 11:02:21
tags: ['face_recognition', '人脸识别']
summary:
---
`face_recognition`是一个强大、简单、易上手的人脸识别开源项目
<a name="0187de94"></a>
### 安装face_recognition
```
brew install cmake
brew install boost
brew install boost-python
pip install face_recognition
```
<a name="5b921a96"></a>
### 安装PIL
```
pip install -U Pillow
```
<a name="1a63ac23"></a>
### 示例
原图<br />![test03.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1551942858045-3259b84e-54c3-414f-ae48-df1b6252006d.jpeg#align=left&display=inline&height=300&name=test03.jpg&originHeight=480&originWidth=640&size=47156&status=done&width=400)<br />代码
```python
#!/usr/bin/python
# -*- coding: utf-8 -*-
import face_recognition
from PIL import Image, ImageDraw

# 加载图片
image = face_recognition.load_image_file("./img/test03.jpg")
# 查找面部
face_locations = face_recognition.face_locations(image,model="cnn")
# 面部数量
faceNum = len(face_locations)
# 读取原图，准备做标志
pil_image = Image.fromarray(image)
for i in range(0, faceNum):
  	# 面部区域的四个坐标
    top =  face_locations[i][0]
    right =  face_locations[i][1]
    bottom = face_locations[i][2]
    left = face_locations[i][3]
    # 画图
    d = ImageDraw.Draw(pil_image, 'RGBA')
    d.rectangle((left, top, right, bottom),outline = "#00FF00")

# 显示标志后的图片
pil_image.show()
```
识别后<br />![20190307151506.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1551942922325-bf9dd7c9-3df2-4797-b6be-9691e2cfa7eb.jpeg#align=left&display=inline&height=310&name=20190307151506.jpg&originHeight=954&originWidth=1232&size=169036&status=done&width=400)