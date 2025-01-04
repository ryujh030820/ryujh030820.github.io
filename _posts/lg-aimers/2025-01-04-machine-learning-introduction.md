---
published: true
title: "[LG Aimers] Machine Learning 개론"
date: 2025-01-04 11:35:00 +0900
categories: [Lecture, LG Aimers]
tags: [lg aimers, ai]
math: true
---
# 1. Introduction to Machine Learning

## 기계학습(Machine Learning)이란?

인공지능의 한 분야로써 실험적으로 얻은 Data로부터 점점 개선될 수 있도록 하는 알고리즘을 설계하고 개발하는 것

![1.png](/assets/img/lg-aimers/machine-learning-introduction/1.png)

### Tom Mitchell의 정의

톰 미첼은 기계학습 알고리즘을 E와 P, T로 정의했다.

- **T(ask)** : 기계학습을 가지고 어떤 작업(ex: 분류, 회귀 등)을 할 것인지
- **P(erformance Measure)** : 어떤 성능 지표(ex: error rate, accuracy 등)를 사용해 평가할 것인지
- **E(xperience)** : 어떤 Data를 활용할 것인지

## Generalization

- 학습의 정의
    - 특정 사례의 공통된 속성을 일반 개념 또는 주장으로 공식화하는 추상화의 한 형태
    - 여러 개별 객체의 유사성 분석을 기반으로 개념의 본질을 추출
- 학습의 목표
    - 훈련 데이터 자체를 정확히 재현하는 것이 아님
    - 데이터를 생성하는 프로세스의 통계적 모델을 구축하는 것

## No Free Lunch Theorem for ML

- 어떤 기계학습 알고리즘도 다른 기계학습 알고리즘보다 항상 좋다고는 할 수 없음
- 매번 새로운 Task, 혹은 새롭게 Data를 모을 때마다 최적의 알고리즘을 찾는 과정을 수행해야 함

## 기계학습의 종류

### 1. 지도 학습(Supervised Learning)

- 기계학습 알고리즘에게 특정 input이 들어오면 특정 output이 나와야 한다고 명시적으로 가르쳐주는 것
- 대표적으로 Classification과 Regression이 있음
    - **분류(Classification)** : y가 범주형일 때
    - **회귀(Regression)** : y가 실수형일 때

![2.png](/assets/img/lg-aimers/machine-learning-introduction/2.png)

### 2. 비지도 학습(Unsupervised Learning)

- 학습 데이터가 x로만 구성
- Clustering, Anomaly Detection, Density Estmation 등이 있음

### 3. 준지도 학습(Semi-supervised Learning)

- 지도 학습과 비지도 학습의 중간 형태
- x와 y를 줄 때, 몇몇 Data들은 그냥 x만 주는 것
- 준지도 학습에서는 주로 2개의 시나리오를 고려함
    - **LU learning** : 일부 데이터에는 사람이 레이블링(y를 제공)하고, 일부 데이터에는 레이블링을 하지 않음
    - **PU learning** : 긍정 데이터(Positive Data)는 긍정적이라고 알려주지만, 부정적인 예시(Negative Example)에 대해서는 학습 단계에서 아무런 레이블을 주지 않는 경우

![3.png](/assets/img/lg-aimers/machine-learning-introduction/3.png)

### 4. 강화 학습(Reinforcement Learning)

- 에이전트가 환경과 상호작용하며 보상을 통해 학습
- 주요 특징:
    - 상태(State), 행동(Action), 보상(Reward) 기반 학습
    - 지연된 보상(Delayed Reward) 처리

---

# 2. Bias and Variance

## 과적합(Overfitting)과 과소적합(Underfitting)

- **과적합**:
    - 모델이 학습 데이터에 지나치게 적합하여 새로운 데이터에서 성능 저하
    - **특징**: 일반화 에러가 학습 에러보다 큼
- **과소적합**:
    - 모델이 학습 데이터조차 제대로 학습하지 못한 상태
    - **특징**: 학습 에러가 너무 큼

## 정규화(Regularization)

- **개념**:
    - 모델의 복잡도를 제한하여 과적합 방지
    - **목적함수**:
        
        $J(w) = error + \lambda w^{T}w$
        
    - **하이퍼파라미터 λ**:
        - $\lambda = 0$ : 정규화 없음
        - $\lambda > 0$ : 모델 복잡도 감소
- **효과**:
    - 학습 에러가 약간 증가하더라도 테스트 에러를 줄임

## 편향(Bias)과 분산(Variance)

- **편향(Bias)**:
    - 예측값의 평균과 실제값의 차이
    - 높은 편향은 학습 데이터의 패턴을 제대로 학습하지 못한 상태
- **분산(Variance)**:
    - 예측값들이 얼마나 분산되어 있는지
    - 높은 분산은 학습 데이터에 지나치게 의존하는 상태
- **Trade-off**:
    - 복잡도가 증가하면 편향은 감소하지만 분산은 증가

![4.png](/assets/img/lg-aimers/machine-learning-introduction/4.png)

## 모델의 용량과 Occam’s Razor

- **모델 용량**: 모델이 다양한 패턴을 학습할 수 있는 능력
- **Occam’s Razor**:
    - 동일한 조건에서 가장 단순한 해법이 최선