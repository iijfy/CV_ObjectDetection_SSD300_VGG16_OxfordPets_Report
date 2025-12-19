# 🐶🐱 SSD 기반 개/고양이 얼굴(Object Detection) 검출

---

📌 1. 프로젝트 목적
Oxford-IIIT Pet 데이터셋의 XML 바운딩박스를 이용해,
SSD300(VGG16 backbone) 모델로 개/고양이 얼굴 영역을 감지(Object Detection)하는 프로젝트입니다.

이 프로젝트는 단순히 모델을 학습하는 것에서 끝나지 않고,
데이터 정합성 확인 → 파이프라인 구성 → 학습 → mAP 평가 → 테스트 시각화까지
탐지 문제를 end-to-end로 재현 가능한 형태로 정리하는 것을 목표로 했습니다.

---

## 데이터
- The Oxford-IIIT Pet Dataset (Kaggle mirror)
- 구성
  - images: 원본 이미지
  - annotations/xmls: 바운딩박스 XML
  - trainval.txt / test.txt: 분할 정보
- 노트북 로그 기준
  - Train/Validation: 3680
  - Test: 3669
  - XML: 3686

---

## 접근 방법
- SSD300(VGG16)
  - torchvision에서 제공하는 표준 구현을 사용해 안정적으로 학습/추론 가능
- Full Fine-tuning
  - 사전학습 가중치 기반에서 전체 파라미터를 미세조정하여 빠른 수렴 기대
- 클래스 불균형 대응
  - WeightedRandomSampler로 샘플링 분포를 보정
- 평가 지표
  - IoU + AP + mAP@0.5
  - 탐지는 단순 정확도보다 mAP가 목적(검출 품질)에 더 직접적

---  

## 핵심 결과
- Epoch 1: Val mAP 0.1919
- Epoch 5: Val mAP 0.9488
- Epoch 10: Val mAP 0.9784
- Epoch 34: Best Val mAP 0.9939
- Epoch 50: Val mAP 0.9930
- 최종 Best mAP: 0.9939

---

## 결론
SSD 기반 얼굴 탐지 파이프라인을 end-to-end로 구성했고,
mAP@0.5 기준 최고 0.9939 성능을 확인했습니다.
또한 AP/mAP 평가 로직을 직접 구현하여 결과의 납득 가능성과 재현성을 확보했습니다.


