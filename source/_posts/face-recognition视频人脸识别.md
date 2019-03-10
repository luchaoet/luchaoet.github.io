---
title: face_recognition视频人脸识别
date: 2019-03-10 12:18:41
tags: ['face_recognition', '人脸识别']
summary:
---
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import face_recognition
import cv2

video_capture = cv2.VideoCapture(0)

# 本地图像
lc_image = face_recognition.load_image_file("./img/test04.jpg")
lc_face_encoding = face_recognition.face_encodings(lc_image)

face_locations = []
face_encodings = []
face_names = []
process_this_frame = True

while True:
    # 读取摄像头画面
    ret, frame = video_capture.read()
    # 改变摄像头图像的大小，图像小，所做的计算就少
    small_frame = cv2.resize(frame, (0, 0), fx=0.25, fy=0.25)

    # opencv的图像是BGR格式的，而我们需要是的RGB格式的，因此需要进行一个转换。
    rgb_small_frame = small_frame[:, :, ::-1]

    # Only process every other frame of video to save time
    # if process_this_frame:
        # 根据encoding来判断是不是同一个人，是就输出true，不是为flase
    face_locations = face_recognition.face_locations(rgb_small_frame)
    face_encodings = face_recognition.face_encodings(rgb_small_frame, face_locations)
    # print(type(face_encodings))

    l = len(face_encodings)
    for i in range(0, l):
        match = face_recognition.compare_faces(lc_face_encoding, face_encodings[i])
        

        top = face_locations[i][0]*4
        right = face_locations[i][1]*4
        bottom = face_locations[i][2]*4
        left = face_locations[i][3]*4
        font = cv2.FONT_HERSHEY_DUPLEX
        match = face_recognition.compare_faces(lc_face_encoding, face_encodings[i])
        name = ''
        if match[0]:
            # 矩形框
            cv2.rectangle(frame, (left, top), (right, bottom), (0, 0, 255), 2)
            #加上标签
            cv2.rectangle(frame, (left, bottom - 35), (right, bottom), (0, 0, 255), cv2.FILLED)
            name = 'luchao'
        else:
            # 矩形框
            cv2.rectangle(frame, (left, top), (right, bottom), (50,205,50), 2)
            #加上标签
            cv2.rectangle(frame, (left, bottom - 35), (right, bottom), (50,205,50), cv2.FILLED)
            name = 'Unknow'
        cv2.putText(frame, name, (left + 6, bottom - 6), font, 1.0, (255, 255, 255), 1)
        
        # print(i,name,match[0])

    # Display
    cv2.imshow('Video', frame)

    # 按Q退出
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

video_capture.release()
cv2.destroyAllWindows()
```