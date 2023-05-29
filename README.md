# AWS-image-recognition-api
Project Name : Image detection by using AWS recognition api

## Description : 설명
이번 프로젝트는 S3에 업로드한 이미지를 업로드한 후
AWS의 Rekognition의 object detection(detect label) 서비스를 활용한다. 

업로드한 이미지상의 객체를 인식하고 그 결과값을 JSON으로 파싱하여 S3에 업로드한 이미지에 네모박스를 표시하도록 한다.
이때, 전방에 위치한 객체에 대해서만 표시하는 기능을 구현한다.
