# Edge-AI-with-YOLO

An Edge AI project for developing an object detection model that can be deployed on edge devices.

[한국어 🇰🇷](README.ko.md)

---

## Purpose

- Design a lightweight YOLO-based model that can run on edge devices
- Apply model optimization techniques such as:
  - Reviewing relevant research papers
  - Reducing network layers
  - Applying parallel processing using the OpenMP API

---

## Members

- Solo project

---

## Tech

- Docker  
- CNN  
- YOLO  
- OpenMP  
- C language  

---

## Project Plan

- Week 1: Paper analysis (one YOLOv3 paper and one Edge AI paper)
- Week 2: Presentation of analysis results and idea sharing
- Week 2: Environment setup using Docker (image, container, development environment)
- Week 3: Testing using source code and dataset (MS-COCO) introduced in the papers
- Week 4: Development of an object detection model using customized data

---

## Paper Review

- Referenced papers:
  - Deep Learning–Based Object Detection Software Development for Edge Computing Boards
  - *YOLOv3: An Incremental Improvement*
  - *On-Device Machine Learning: An Algorithms and Learning Theory Perspective*

---

## Paper Presentation and Idea Sharing

- Presentation flow:
  - Introduction to edge computing software
  - Explanation of YOLOv3
    - Anchor box construction
    - Bounding box prediction
    - Class prediction
    - Feature extraction

---

## Development Environment Setup

- Docker-based development environment configuration
  - NVIDIA driver installation
  - CUDA installation compatible with the driver version

- YOLOv3 environment setup
  - Clone Darknet
    ```bash
    git clone https://github.com/pjreddie/darknet
    ```
  - Modify the Makefile
    ```makefile
    GPU=1
    CUDNN=0
    OPENCV=0
    OPENMP=0
    DEBUG=0

    ARCH= -gencode arch=compute_35,code=sm_35 \
          -gencode arch=compute_50,code=[sm_50,compute_50] \
          -gencode arch=compute_52,code=[sm_52,compute_52] \
          -gencode arch=compute_70,code=[sm_70,compute_70] \
          -gencode arch=compute_75,code=[sm_75,compute_75] \
          -gencode arch=compute_80,code=[sm_80,compute_80]
    ```
  - Download pre-trained weight file
    ```bash
    wget https://pjreddie.com/media/files/yolov3.weights
    ```
  - Sample test
    ```bash
    ./darknet detect cfg/yolov3.cfg yolov3.weights data/dog.jpg
    ```

---

## Training Data Preparation (MS-COCO)

- Clone COCO API
  ```bash
  git clone https://github.com/pdollar/coco
  cd coco
  mkdir images
  cd images

## Download image datasets
```
wget -c https://pjreddie.com/media/files/train2014.zip
wget -c https://pjreddie.com/media/files/val2014.zip
unzip -q train2014.zip
unzip -q val2014.zip
cd ..
```

## Download COCO metadata (train.txt, valid.txt, labels)
```
wget -c https://pjreddie.com/media/files/instances_train-val2014.zip
wget -c https://pjreddie.com/media/files/coco/5k.part
wget -c https://pjreddie.com/media/files/coco/trainvalno5k.part
wget -c https://pjreddie.com/media/files/coco/labels.tgz
tar xzf labels.tgz
unzip -q instances_train-val2014.zip
```

## Generate train.txt and valid.txt
```
paste <(awk "{print \"$PWD\"}" <5k.part) 5k.part | tr -d '\t' > valid.txt
paste <(awk "{print \"$PWD\"}" <trainvalno5k.part) trainvalno5k.part | tr -d '\t' > train.txt
```

## Dataset structure
```
├── images
│   ├── train2014
│   └── val2014
├── labels
│   ├── train2014
│   └── val2014
├── train.txt
└── valid.txt
```

## Download initial weights for training
```wget https://pjreddie.com/media/files/darknet53.conv.74```

## Model Configuration for Lightweight Optimization
```
[net]
batch=64
subdivisions=1
width=256
height=256
channels=3
momentum=0.9
decay=0.0005

learning_rate=0.001
policy=steps
burn_in=1000
max_batches=160200
steps=128000,144000
scales=.1,.1

[yolo]
mask=0,1,2
anchors=21,20, 50,47, 115,107
classes=80
num=3
random=1
```

## Start training
```
./darknet detector train cfg/coco.data cfg/YOLOv3-edge.cfg darknet53.conv74 -gpus 1,2 | tee backup/train.log
```
