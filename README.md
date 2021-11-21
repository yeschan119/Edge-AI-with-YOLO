# Edge-AI-with-YOLO
edge에서 구현 가능한 AI model(object detection) 개발 프로젝트

## Purpose
  + Yolo 모델을 경량화 하여 edge에서 구현 가능하도록 설계
  + 경량화 방법은
    + 해당 논문리뷰 진행
    + layer를 줄이는 방법 고려
    + openMP API를 사용하여 병렬처리 진행

## Members
  + 1인 프로젝트

## Tech
  + docker
  + CNN
  + YOLO
  + openMP
  + C-language

## Project Plan
  + 1주차 논문 분석(YOLOv3 논문과 edge AI 논문 각 1부씩)
  + 2주차 분석한 내용을 정리하여 발표 및 아이디어 공유
  + 2주차 docker를 이용하여 이미지, 컨테이너 설치 및 개발환경 구성
  + 3주차 논문에서 소개한 소스코드와 데이터셋(ms-coco) 이용한 테스팅 진행
  + 4주차 customized data를 이용한 AI객체 인식 모델 개발

## 논문 분석 진행
  + 참조논문
    + Edge Computing board 탑재용 딥러닝 기반 객체 인식 SW 개발
    + YOLOv3 An Incremental Improvement
    + On-Device Machine Learning: An Algorithms and Learning Theory Perspective
## 논문 내용 발표 및 아이디어 공유
  + 발표 순서
    + Edge computing software 소개
    + YOLOv3 설명
      + anchor box build
      + bounding box prediction
      + class prediction
      + feature extract
## 개발환경 구축
  + docker 개발환경 설정
    + nvidia driver 설치
    + 버전에 맞는 cuda 설치
  + YOLOv3 사용환경 구축
    + darknet 내려 받기
      ```
      git clone https://github.com/pjreddie/darknet
      ```
    + MakeFile 수정
      ```
      GPU=1
      CUDNN=0

      OPENCV = 0

      OPENMP=0

      DEBUG=0

      ARCH= -gencode arch=compute_35,code=sm_35 \
      -gencode arch=compute_50,code=[sm_50,compute_50] \
      -gencode arch=compute_52,code=[sm_52,compute_52] \
      -gencode arch=compute_70,code=[sm_70,compute_70] \
      -gencode arch=compute_75,code=[sm_75,compute_75] \
      -gencode arch=compute_80,code=[sm_80,compute_80] \
      -gencode arch=compute_80,code=[sm_80,compute_80]
      ```
    + download pre-trained weight file
      ```
      wget https://pjreddie.com/media/files/yolov3.weights
      ```
    + sample test
      ```
      ./darknet detect cfg/yolov3.cfg yolov3.weights data/dog.jpg
      ```
      ![dog](https://user-images.githubusercontent.com/83147205/142766118-aa23d0fe-ee82-49bc-9283-40200faa3c5f.png)
      
    + training data 구성(ms-coco)
      
      + clone coco API
        ```
        - git clone https://github.com/pdollar/coco
        - cd coco
        - mkdir images
        - cd images
        ```
      + download images
        ```
        - wget -c https://pjreddie.com/media/files/train2014.zip
        - wget -c https://pjreddie.com/media/files/val2014.zip
        - unzip -q train2014.zip
        - unzip -q val2014.zip
        - cd ..
        ```
      + download coco meta data(trian.txt, valid.txt, labels)
        ```
        - wget -c https://pjreddie.com/media/files/instances_train-val2014.zip
        - wget -c https://pjreddie.com/media/files/coco/5k.part
        - wget -c https://pjreddie.com/media/files/coco/trainvalno5k.part
        - wget -c https://pjreddie.com/media/files/coco/labels.tgz
        - tar xzf labels.tgz
        - unzip -q instances_train-val2014.zip
        ```
      + train.txt, valid.txt 파일 생성
        ```
        - paste <(awk "{print \"$PWD\"}" <5k.part) 5k.part | tr -d '\t' > valid.txt
        - paste <(awk "{print \"$PWD\"}" <trainvalno5k.part) trainvalno5k.part | tr -d '\t' > train.txt
        ```
      + dataset 구조 구성
        ```
        ├── images

        │   ├── train2014

        │   │   ├── COCO_train2014_000000000009.jpg

        │   │   ├── COCO_train2014_000000000025.jpg

        │   │   └── ...

        │   └── val2014

        │       ├── COCO_val2014_000000000042.jpg

        │       ├── COCO_val2014_000000000073.jpg

        │       └── ...

        ├── labels

        │   ├── train2014

        │   │   ├── COCO_train2014_000000000009.txt

        │   │   ├── COCO_train2014_000000000025.txt

        │   │   └── ...

        │   └── val2014

        │       ├── COCO_val2014_000000000042.txt

        │       ├── COCO_val2014_000000000073.txt

        │       └── ...

        ├── train.txt

        └── valid.txt
        ```
      + training을 위한 가중치 다운로드
        ```
        wget https://pjreddie.com/media/files/darknet53.conv.74
        ```
        
## 모델 경량화를 위한 config 파일 수정
    ```
    [net]
    ######## Testing
    #batch=1
    #subdivisions=1
    ######## Training
    batch=64
    subdivisions=1
    width=256
    height=256
    channels=3
    momentum=0.9
    decay=0.0005
    angle=0
    saturation = 1.5
    exposure = 1.5
    hue=.1

    learning_rate=0.001
    policy=steps
    burn_in=1000
    max_batches = 160200
    steps=128000,144000
    scales=.1,.1

    [yolo]
    mask = 0,1,2
    anchors = 21,20, 50,47, 115,107
    classes=80
    num=3
    jitter=.3
    ignore_thresh = .5
    truth_thresh = 1
    random=1
    ```
   + 학습 시작
   
    ```
    ./darknet detector train cfg/coco.data cfg/YOLOv3-edge.cfg darknet53.conv74 -gpus 1,2 | tee backup/train.log
    ```
   + 학습 결과 확인
   + ![predictions](https://user-images.githubusercontent.com/83147205/142768393-a33d5564-0b5b-4d1c-a89c-6b51c0dc317b.jpg)
   + 
    
