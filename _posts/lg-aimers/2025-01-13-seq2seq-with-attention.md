---
published: true
title: "[LG Aimers] [딥러닝-4] Seq2Seq with Attention for Natural Language Understanding and Generation"
date: 2025-01-13 13:18:00 +0900
categories: [Lecture, LG Aimers]
tags: [lg aimers, ai]
math: true
---
## Recurrent Neural Networks

### Recurrent Neural Network (RNNs)

- 주어진 순차 데이터에 대해 동일한 함수를 시간에 따라 재귀적으로 실행함
- 벡터의 시퀀스 x를 다음의 재귀 공식(Recurrence Formula)을 각 시간 단계에 적용하여 처리할 수 있음
    
    $h_t = \tanh{(W_{hh}H_{t-1}+W_{xh}x_t)}$
    
- 옵션으로 특정 시간 단계에서 목표 변수(target variable)를 예측할 때 $h_t$를 출력층의 입력으로 사용할 수 있음

![1.png](/assets/img/lg-aimers/seq2seq-with-attention/1.png)

### Various Problem Settings of RNN-based Sequence Modeling

![2.png](/assets/img/lg-aimers/seq2seq-with-attention/2.png)

- one-to-one: Vanilla Neural Networks
- one-to-many: 이미지 캡션
- many-to-one: 감정 분석
- many-to-many: 기계 번역

### Character-level Language Model

![3.png](/assets/img/lg-aimers/seq2seq-with-attention/3.png)

- Character-level language model 예시:
- Vocabulary: [h, e, l, o]
- 예시 training sequence: “hello”
- 테스트 시간에 한 번에 하나씩 문자를 샘플링하여 다음 시간 단계에서 모델에 입력으로 피드백을 제공하는데, 이를 **자동 회귀 모델(auto-regressive model)**이라고 함

### Long Short-Term Memory (LSTM)

![4.png](/assets/img/lg-aimers/seq2seq-with-attention/4.png)

- Vanishing & exploding gradients 문제를 완하시키기 위해 디자인됨
- Hidden state와 Cell state를 사용

## Seq2Seq and Attention Model

### Seq2Seq Model

- 기능: 단어 시퀀스를 입력으로 받아 단어 시퀀스를 출력으로 반환함
- 구성: 인코더(Encoder)와 디코더(Decoder)로 구성됨
- 적용 분야: 기계 번역, 챗봇 등 다양한 자연어 처리(NLP) 응용 프로그램에 사용됨
- 작동 원리:
    - 입력 소스 시퀀스를 고정된 차원의 벡터로 인코딩
    - 인코딩된 벡터를 디코더 모델의 초기 은닉 상태 벡터 $h_0$로 사용

![5.png](/assets/img/lg-aimers/seq2seq-with-attention/5.png)

### Seq2Seq Attention

![6.png](/assets/img/lg-aimers/seq2seq-with-attention/6.png)

- Attention 출력: 인코더 상태를 Attention 가중치로 가중합한 결과
- Attention 가중치: 소스 토큰의 분포
- 특징:
    - 모델이 각 시간 단계마다 가장 관련 있는 소스 토큰에 “집중”하도록 학습 가능

### Attention의 장점

- Attention은 기계 번역(NMT) 성능을 크게 향상시킴
- Attention은 병목 현상(Bottleneck Problem)을 해결함:
    - 디코더가 소스 시퀀스를 직접 참조할 수 있도록 하여 병목 문제를 완화함
- Attention은 기울기 소실 문제(Vanishing Gradient Problem)을 완화함
    - 먼 상태(faraway states)에 대한 지름길을 제공함