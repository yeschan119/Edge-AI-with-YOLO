# Edge-AI-with-YOLO

An Edge AI project focused on developing an object detection model that can run efficiently on resource-constrained edge devices.

[한국어 🇰🇷](README.ko.md)

---

## Purpose

- Design a lightweight YOLO-based model suitable for edge deployment
- Apply model optimization techniques including:
  - Reviewing relevant research papers
  - Reducing network layers
  - Parallel processing using the OpenMP API

---

## Members

- Solo project

---

## Tech Stack

- Docker  
- CNN  
- YOLO  
- OpenMP  
- C language  

---

## Project Plan

- **Week 1**  
  - Paper review (YOLOv3 paper + one Edge AI–related paper)
- **Week 2**  
  - Summarize analysis results and present ideas  
  - Set up Docker images, containers, and development environment
- **Week 3**  
  - Testing using source code and dataset (MS-COCO) introduced in the paper
- **Week 4**  
  - Develop an object detection model using customized data

---

## Paper Review

### Referenced Papers

- Deep Learning–Based Object Detection Software Development for Edge Computing Boards  
- *YOLOv3: An Incremental Improvement*  
- *On-Device Machine Learning: An Algorithms and Learning Theory Perspective*

---

## Paper Presentation & Idea Sharing

### Presentation Agenda

- Introduction to edge computing software
- YOLOv3 overview
  - Anchor box construction
  - Bounding box prediction
  - Class prediction
  - Feature extraction

---

## Development Environment Setup

### Docker Environment Configuration

- NVIDIA driver installation
- CUDA installation compatible with driver version

### YOLOv3 Environment Setup

- Clone Darknet repository
  ```bash
  git clone https://github.com/pjreddie/darknet
