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
![img](https://github.com/dude1599/Image-recognition-detection-service/assets/133233495/2789dead-c300-4bda-9288-b06dd1b98b8c)

## AWS에서 제공하는 detect label을 돌려봤을 경우 결과값들 사진
![스크린샷 2023-06-19 160236](https://github.com/dude1599/Image-recognition-detection-service/assets/133233495/9ec9b1d2-2f04-435a-ae36-073ea454eaa8)


## detect label을 사용한 코드
-----------------------------------------------------------------------------------------------
<pre>
<code>
const AWS = require('aws-sdk'); // Import required packages
const fs = require('fs');
const Jimp = require('jimp');

AWS.config.update({
  accessKeyId: 'AKIAU4CT3UBRVRYWPV2M',
  secretAccessKey: '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',
  region: 'ap-northeast-2'
});  // AWS access details

const s3 = new AWS.S3();  // Create an instance of the S3 service

const bucketName = 'portensia1testbucket';
const filePath = 'D:\\rekognition\\img.jpg';   // Specify the bucket name and image file path

const fileData = fs.readFileSync(filePath);   // Read the image file

const uploadParams = {          
  Bucket: bucketName,
  Key: 'img.jpg',
  Body: fileData
};                                            // Set the parameters for S3 upload

s3.upload(uploadParams, async function(err, data) {
  if (err) {
    console.log('Error uploading image:', err);
  } else {
    console.log('Image uploaded successfully:', data.Location);
  }                                          // Upload the image file to S3

 
  const rekognition = new AWS.Rekognition();         // Call AWS Rekognition class

  const detectLabelsParams = {
    Image: {
      S3Object: {
        Bucket: bucketName,
        Name: 'img.jpg'
      }
    },
    MaxLabels: 5
  };                                   // Set the parameters for Rekognition DetectLabels API

  rekognition.detectLabels(detectLabelsParams, async function(err, data) {
    if (err) {
      console.log('Error detecting labels:', err);
    } else {
      console.log('Labels:', data.Labels);       // Call Rekognition DetectLabels API

</code>
</pre>

  
## detect label을 사용하여 얻은 결과. (위 사진의 라벨만 식별했을 경우)
----------------------------------------------------------------------------------------------------------
<pre><code>
  Labels: [
  {
    Name: 'Pedestrian',
    Confidence: 99.97284698486328,
    Instances: [],
    Parents: [ [Object] ],
    Aliases: [],
    Categories: [ [Object] ]
  },
  {
    Name: 'Person',
    Confidence: 99.97284698486328,
    Instances: [
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object]
    ],
    Parents: [],
    Aliases: [ [Object] ],
    Categories: [ [Object] ]
  },
  {
    Name: 'Boy',
    Confidence: 98.53184509277344,
    Instances: [ [Object] ],
    Parents: [ [Object], [Object] ],
    Aliases: [],
    Categories: [ [Object] ]
  },
  {
    Name: 'Male',
    Confidence: 98.53184509277344,
    Instances: [ [Object] ],
    Parents: [ [Object] ],
    Aliases: [],
    Categories: [ [Object] ]
  },
  {
    Name: 'Teen',
    Confidence: 98.53184509277344,
    Instances: [ [Object] ],
    Parents: [ [Object] ],
    Aliases: [],
    Categories: [ [Object] ]
  }
]
</code></pre>

  
  ## Jimp를 사용하여 이미지 파일에 경계 박스 그리는 방법 소개 (코드 && 결과 - 사진 첨부)
  -----------------------------------------------------------------------------------------------
  위에서 첨부한 이미지 파일에 경계박스를 그리는 예제 연습 및 결과 소개
  (첨부한 이미지의 가로, 세로 길이는 1333 px이다)
  
  const Jimp = require('jimp');
  const imagePath = 'C:\\Users\\이희윤\\Desktop\\이희윤\\img.jpg';  // 이미지 경로
Jimp.read(imagePath)
  .then(image => {
    // 이미지의 너비와 높이 가져오기
    const width = image.getWidth();
    const height = image.getHeight();

    // 이미지의 가로, 세로길이 출력
    console.log('가로 길이:', width);
    console.log('세로 길이:', height);

    // 네모박스 그리기
    const borderWidth = 20; // 경계선 두께
    const boxWidth = 600         // 박스 가로 세로
    const boxHeight = 600
    const topLeftX = 280            // 왼쪽 상단 x,y 좌표
    const topLeftY = 400
    const bottomRightX = 1000       // 오른쪽 하단 x,y좌표
    const bottomRightY = 1180

    // 네모박스 경계선 그리기
    for (let x = topLeftX; x <= bottomRightX; x++) {
      for (let y = topLeftY; y <= bottomRightY; y++) {
        if (x < topLeftX + borderWidth || x > bottomRightX - borderWidth || y < topLeftY + borderWidth || y > bottomRightY - borderWidth) {
          // 경계선 색깔 설정 (빨간색)
          image.setPixelColor(Jimp.rgbaToInt(255, 0, 0, 255), x, y);
        }
      }
    }
    // 이미지 저장
    image.write('output.jpg');
  })
  .catch(err => {
    console.error(err);
  });



   

  
