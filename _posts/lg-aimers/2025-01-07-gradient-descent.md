---
published: true
title: "[LG Aimers] [지도학습-3] Gradient Descent"
date: 2025-01-07 19:06:00 +0900
categories: [Lecture, LG Aimers]
tags: [lg aimers, ai]
math: true
---
## 지도학습 재방문

- **문제 설정:**
    - 주어진 데이터셋 $x_i, y_i$.
    - **함수 클래스:** $f_\theta(x)$.
    - **손실 함수:** $L(f_\theta(x), y)$.
    - 목표: 손실 $L$을 최소화하는 파라미터 $\theta$를 찾는 것.

## 최소화 문제

- **기본 수학적 접근:**
    - 손실 함수를 최소화하기 위해 미분값이 0이 되는 지점(극값)을 찾음:
    $\nabla_\theta L(\theta) = 0$
    - 이론적으로 가능하지만, 복잡한 함수에서는 해석적(analytic) 해를 구하기 어려움.

## 경사하강법 (Gradient Descent)

- **가정:**
    - 함수의 기울기$(\nabla_\theta L)$를 계산할 수 있음.
    - 전역적인 정보를 알 수 없으므로, 기울기에 따라 한 번에 한 걸음씩 진행.
- **알고리즘:**
    1. 초기값 $\theta_0$ 설정 (랜덤 초기화).
    2. 반복 과정:
    $\theta_{t+1} = \theta_t - \alpha \nabla_\theta L(\theta_t)$
        - $\alpha$: 학습률(learning rate).
- **학습률 설정:**
    - 너무 작으면 수렴 속도가 느림.
    - 너무 크면 발산할 위험이 있음.

## 학습률 (Learning Rate)

- **적절한 학습률의 중요성:**
    - 학습률이 너무 작으면 느리게 수렴.
    - 학습률이 너무 크면 최소값을 넘어서거나 발산.
- **실제 사례:**
    - 학습 속도 조절을 위해 학습률 스케줄링 사용 가능.

## 스토캐스틱 경사하강법 (SGD)

- **문제점:**
    - 전체 데이터셋에 대해 기울기를 계산하는 데 시간이 오래 걸릴 수 있음.
- **해결책:**
    - 데이터셋의 일부(배치)를 사용하여 기울기 계산.
    - **SGD 알고리즘:**
    $\theta_{t+1} = \theta_t - \eta \nabla_\theta L(\theta_t; x_i, y_i)$
        - $x_i, y_i$: 미니배치의 샘플 데이터.
    - 데이터셋이 크더라도 빠르게 학습 가능.

## 경사하강법의 한계

- **국소 최소점 (Local Minima):**
    - 전역 최소값(global minima) 대신 국소 최소값(local minima)에 갇힐 수 있음.
- **평평한 지역 (Plateau):**
    - 기울기가 거의 0인 영역에서 학습 속도가 느려짐.

## 경사하강법 변형

- **모멘텀 (Momentum):**
    - 이전 단계의 기울기를 활용하여 최적화 속도를 높임.
    - 업데이트 공식:
    $v_t = \beta v_{t-1} + (1 - \beta)\nabla_\theta L(\theta_{t-1})$
    $\theta_{t+1} = \theta_t - \alpha v_t$
    - $\beta$: 모멘텀 계수 (보통 0.9 사용).
- **RMSProp:**
    - 각 변수의 변화량에 따라 학습률을 조정.
    - 공식:
    $s_t = \gamma s_{t-1} + (1-\gamma) \nabla_\theta^2 L$
    $\theta_{t+1} = \theta_t - \frac{\eta}{\sqrt{s_t + \epsilon}} \nabla_\theta L$
- **Adam (Adaptive Moment Estimation):**
    - 모멘텀과 RMSProp을 결합한 최적화 알고리즘.
    - 두 가지 모멘트를 사용:
        - 1차 모멘트(기울기의 평균).
        - 2차 모멘트(기울기의 분산).
    - 업데이트 공식:
    $\theta_{t} = \theta_{t-1} - \frac{\alpha \cdot \hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon}$

## 학습률 스케줄링

- **지수 스케줄링:** 학습률을 점진적으로 감소시킴.
$\alpha_t = \alpha_0 \cdot e^{-\lambda t}$
- **적응형 스케줄링:** 성능 변화에 따라 학습률을 동적으로 조정.

## 요약

- 경사하강법의 핵심:
    - 기울기를 사용하여 손실 함수 최소화.
    - 적절한 학습률과 초기화를 통해 성능 최적화.
- 다양한 변형(모멘텀, RMSProp, Adam 등)이 경사하강법의 단점을 보완.
- 학습률 스케줄링 및 데이터 샘플링(SGD)을 통해 효율적으로 학습.