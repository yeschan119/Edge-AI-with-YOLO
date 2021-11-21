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
  + 3주차 논문에서 소개한 소스코드와 데이터셋(ms-coco)를 이용한 테스팅 진행
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
    + 
    + 
