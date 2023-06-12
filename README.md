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
![test](https://github.com/dude1599/Image-recognition-detection-service/assets/133233495/3817ef2f-34c4-4170-b298-47f560b70cb1)



## detect label을 사용한 코드. (위 사진의 라벨만 식별했을 경우)
<code>
//import required packages
var AWS = require('aws-sdk');

//AWS access details
AWS.config.update({
    accessKeyId: 'ACK',
    secretAccessKey: 'SACK',
    region: 'ap-northeast-2'
  });

  //input parameters
  var params = {
    Image: {
     S3Object: {
      Bucket: "portensia1testbucket", 
      Name: "신호등 사진.jpg"
     }
    },
    MaxLabels: 5,
    MinConfidence: 80
   };

   //Call AWS Rekognition Class
  const rekognition = new AWS.Rekognition();


  //Detect text
  rekognition.detectLabels(params, function(err, data) {
    if (err) console.log(err, err.stack); // an error occurred
    else     console.log(data);           // successful response

  });

// end code
  
  
## detect label을 사용하여 얻은 결과. (위 사진의 라벨만 식별했을 경우)
  Labels: [
  {
    Name: 'Shoe',
    Confidence: 99.7796630859375,
    Instances: [
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object]
    ],
    Parents: [ [Object], [Object] ],
    Aliases: [],
    Categories: [ [Object] ]
  },
  {
    Name: 'Boy',
    Confidence: 99.65534210205078,
    Instances: [ [Object] ],
    Parents: [ [Object], [Object] ],
    Aliases: [],
    Categories: [ [Object] ]
  },
  {
    Name: 'Child',
    Confidence: 99.65534210205078,
    Instances: [ [Object], [Object], [Object] ],
    Parents: [ [Object] ],
    Aliases: [ [Object] ],
    Categories: [ [Object] ]
  },
  {
    Name: 'Male',
    Confidence: 99.65534210205078,
    Instances: [ [Object] ],
    Parents: [ [Object] ],
    Aliases: [],
    Categories: [ [Object] ]
  },
  {
    Name: 'Person',
    Confidence: 99.65534210205078,
    Instances: [ [Object], [Object], [Object], [Object], [Object], [Object] ],
    Parents: [],
    Aliases: [ [Object] ],
    Categories: [ [Object] ]
  }  
  
  ## S3 버킷에 이미지 업로드하는 코드
    
    const fs = require('fs');
    // Create an instance of the S3 service
    const s3 = new AWS.S3();

    // Specify the bucket name and image file path
    const bucketName = 'portensia1testbucket';
    const filePath = 'D:\\rekognition\\test.jpg';

    // Read the image file
    const fileData = fs.readFileSync(filePath);

    // Set the parameters for S3 upload
    const uploadParams = {
      Bucket: bucketName,
      Key: 'test.jpg',
      Body: fileData
    };

    // Upload the image file to S3
    s3.upload(uploadParams, function(err, data) {
      if (err) {
        console.log('Error uploading image:', err);
      } else {
        console.log('Image uploaded successfully:', data.Location);

  
