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
  + 3주차 개발 착수
