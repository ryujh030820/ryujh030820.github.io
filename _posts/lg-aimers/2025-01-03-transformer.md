---
published: true
title: "[LG Aimers] [딥러닝-5] Transformer"
date: 2025-01-13 18:27:00 +0900
categories: [Lecture, LG Aimers]
tags: [lg aimers, ai]
math: true
---
## How Transformer Model Works

### Transformer: High-level View

- Attention module은 seq2seq에서 시퀀스 인코더와 디코더의 역할을 모두 수행할 수 있음
- 즉, RNN이나 CNN은 더 이상 필요하지 않고 Attention module만 있으면 됨

![1.png](/assets/img/lg-aimers/transformer/1.png)

### Long-term Dependency Issue of RNN Models

![2.png](/assets/img/lg-aimers/transformer/2.png)

- 입력 데이터의 step이 길어질수록, context에서 앞선 입력 데이터의 정보가 희미해지는 **장기 의존성 문제**가 발생할 수 있음

### Transformer: Scaled Dot-product Attention

![3.png](/assets/img/lg-aimers/transformer/3.png)

- 쿼리(Q), 키(K), 값(V) 간의 관계를 계산하며,
    
    
    $Attention(Q, K, V) = softmax\left(\frac{QK^T}{\sqrt{d_k}}\right)V$
    
    식을 사용함
    
- $d_k$가 커질수록 softmax 결과가 집중되므로(분산이 커짐으로써), $\sqrt{d_k}$로 스케일링하여 균형을 맞춤

### Transformer: Multi-head Attention

![4.png](/assets/img/lg-aimers/transformer/4.png)

- 단일 Attention으로는 부족한 다양한 관계를 표현하기 위해 여러 개의 Attention 헤드를 사용함
- 각 헤드의 결과를 연결(Concat)하고 선형 변환을 수행함 → 원래 벡터의 차원으로 되돌림

### Transformer: Quadratic Memory Complexity

![5.png](/assets/img/lg-aimers/transformer/5.png)

- $QK^T$를 저장하기 위해서 $n^2$의 메모리 공간이 필요하므로, 시퀀스의 길이가 길어질수록 메모리 복잡도가 커짐

### Transformer: Block-Based Model

![6.png](/assets/img/lg-aimers/transformer/6.png)

- Transformer는 여러 개의 블록으로 구성되며, 각 블록은 다음을 포함함:
    - Multi-head Attention
    - Feed-forward 신경망 (ReLU 활성화 포함)
- 각 블록에는 잔차 연결(Residual Connection) 및 Layer Normalization이 적용됨

### Transformer: Positional Encoding

- 입력 단어 간의 순서를 학습시키기 위해 사인 및 코사인 함수 기반의 위치 정보를 추가함:
    - $PE(pos, 2i) = \sin\left(\frac{pos}{10000^{\frac{2i}{d_{model}}}}\right)$
    - $PE(pos, 2i+1) = \cos\left(\frac{pos}{10000^{\frac{2i}{d_{model}}}}\right)$

### Transformer: Decoder

![7.png](/assets/img/lg-aimers/transformer/7.png)

- 디코더에서는 하위 두 계층이 변경됨
- 이전에 생성된 출력에 대한 마스킹
- Encoder-Decoder attention: Query는 이전 디코더 계층에서 오고 Key와 Value는 인코더의 출력에서 옴

### Transformer: Masked Self-attention

- 디코더는 이전 단계에서 생성된 단어만 참조할 수 있도록 마스킹(Masking)을 사용함
- 이는 다음과 같은 문제를 방지함:
    - 생성되지 않은 단어에 접근하는 문제
    - Softmax 출력의 재정규화를 통해 유효하지 않은 단어 접근을 방지함

![8.png](/assets/img/lg-aimers/transformer/8.png)