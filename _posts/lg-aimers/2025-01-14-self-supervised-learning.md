---
published: true
title: "[LG Aimers] [딥러닝-6] Self-Supervised Learning and Large-Scale Pre-Trained Models"
date: 2025-01-14 13:12:00 +0900
categories: [Lecture, LG Aimers]
tags: [lg aimers, ai]
math: true
---
## Self-Supervised Learning and Large-Scale Pre-Trained Models

### What is Self-Supervised Learning?

- 라벨이 없는 데이터를 활용하여 데이터를 일부 숨기고 나머지를 이용해 이를 예측하는 작업을 설정하여 모델을 학습하는 방법
- 예시)
    - Image Inpainting
    - Jigsaw Puzzle

### Transfer Learning from Self-Supervised Pre-trained Model

- 자기 지도 학습으로 사전 학습된 모델은 특정 작업에 맞게 미세 조정(Fine-tuning)하여 정확도를 향상시킬 수 있음

![1.webp](/assets/img/lg-aimers/self-supervised-learning/1.webp)

### BERT

- **BERT**
    - Pre-training of Deep Bidirectional Transformers for Language Understanding
- 사전 학습 작업
    - 마스킹 언어 모델링(Masked Language Modeling, MLM)
        - 입력 토큰의 15%를 무작위로 마스킹(masking)하여 예측하도록 학습
    - 다음 문장 예측(Next Sentence Prediction, NSP)
        - 주어진 문장이 다른 문장을 논리적으로 이어받는지, 아니면 무관한지 판단

![2.jpg](/assets/img/lg-aimers/self-supervised-learning/2.jpg)

### GPT: Generative Pre-Trained Transformer

- **생성 사전 학습 작업(Generative Pre-Training Task)**
    - 다시 말해, 이 작업은 **언어 모델링(Language Modeling)**이라고 불림
    - 또 다른 관점에서, 이 작업은 **자가 회귀 모델(Auto-Regressive Model)**이라고도 함. 현재 시점에서 예측된 출력값이 다음 시점의 입력값으로 사용된다는 점에서 그러함

![3.png](/assets/img/lg-aimers/self-supervised-learning/3.png)