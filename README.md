# Image-recognition(detection) service
Project Name : Image detection by using AWS recognition api

## Description : 설명
이번 프로젝트는 VScode와 AWS 웹사이트의 AWS Rekognition api를 사용한다.
AWS의 Rekognition의 object detection(detect label) 서비스를 활용하여 이미지를 분석하는 프로그램을 구현한다.

## 구현하고자 하는 기능 (목표)
VScode에서 아래 첨부한 이미지를 S3에 업로드한다.
업로드한 이미지상의 객체를 인식하고 그 결과값을 JSON으로 파싱하여 S3에 업로드한 이미지에 네모박스를 표시하도록 한다.
이때, 전방에 위치한 객체에 대해서만 탐지하고 그 결과를 네모박스로 표시하고 이를 다시 S3에 업로드하는 기능을 구현한다. (Jimp 라이브러리를 사용할 예정)


## S3에 업로드할 이미지
<img src="https://github.com/dude1599/AWS-image-recognition-api/assets/133233495/dace9836-c577-443f-abe4-e9ad3c4b9472" alt="이미지 설명" width="100%" height="50%">
