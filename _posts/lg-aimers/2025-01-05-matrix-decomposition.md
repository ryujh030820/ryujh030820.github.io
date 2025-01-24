---
published: true
title: "[LG Aimers] [수학-1] Matrix Decomposition"
date: 2025-01-15 14:50:00 +0900
categories: [Lecture, LG Aimers]
tags: [lg aimers, ai]
math: true
---
## 1️⃣ Determinant and Trace

### Determinant: Motivation

- 행렬 
$A = \begin{bmatrix}a_{11} & a_{12} \\a_{21} & a_{22}\end{bmatrix}$에 대해:
    - 역행렬 $A^{-1}$은 다음과 같이 정의됨:
        
        
        $A^{-1} = \frac{1}{a_{11}a_{22} - a_{12}a_{21}} \begin{bmatrix} a_{22} & -a_{12} \\ -a_{21} & a_{11} \end{bmatrix}$
        
    - A가 가역(invertible)일 조건: $a_{11}a_{22} - a_{12}a_{21} \neq 0$
- Determinant(행렬식)의 정의:
    - $det(A) = a_{11}a_{22} - a_{12}a_{21}$
- 표기법:
    - $det(A)$ 또는 $|A|$
- 3x3 행렬의 Determinant:
    - 행렬 $A = \begin{bmatrix} a_{11} & a_{12} & a_{13} \\ a_{21} & a_{22} & a_{23} \\ a_{31} & a_{32} & a_{33} \end{bmatrix}$에 대해:
        
        
        $det(A) = a_{11}a_{22}a_{33} + a_{21}a_{32}a_{13} + a_{31}a_{12}a_{23} - a_{31}a_{22}a_{13} - a_{11}a_{32}a_{23} - a_{21}a_{12}a_{33}$
        
    - 이는 **가우스 소거법(Gaussian Elimination)** 또는 다른 대수적 방법으로 계산됨
- 패턴 찾기:
    - $a_{11}a_{22}a_{33} + a_{21}a_{32}a_{13} + a_{31}a_{12}a_{23} - a_{31}a_{22}a_{13} - a_{11}a_{32}a_{23} - a_{21}a_{12}a_{33}$
        
        위 식은 다음과 같이 정리됨:
        
        $a_{11}(-1)^{1+1} \text{det}(A_{1,1}) + a_{12}(-1)^{1+2} \text{det}(A_{1,2}) + a_{13}(-1)^{1+3} \text{det}(A_{1,3})$
        
    - 여기서 여기서 $A_{k,j}$는 행렬 $A$에서 k번째 행과 j번째 열을 제거한 **부분 행렬(Submatrix)**임

### Determinant: Formal Definition

- Determinant의 정의:
    - 정사각 행렬 $A \in \mathbb{R}^{n \times n}$에 대해, $j = 1, \dots, n$에 대해 다음과 같이 정의됨:
        1. 열 j를 기준으로 확장:
            
            
            $\text{det}(A) = \sum_{k=1}^{n} (-1)^{k+j} a_{kj} \text{det}(A_{k,j})$
            
        2. 행 j를 기준으로 확장:
            
            
            $\text{det}(A) = \sum_{k=1}^{n} (-1)^{k+j} a_{jk} \text{det}(A_{j,k})$
            
- 정리(Theorem)
    - $\text{det}(A) \neq 0 \iff \text{rk}(A) = n \iff A$는 **가역(Invertible)**임

### Determinant: Properties

1. **곱의 행렬식**
$\text{det}(AB) = \text{det}(A) \cdot \text{det}(B)$
    - 행렬 곱의 행렬식은 각 행렬의 행렬식을 곱한 것과 같음
2. **전치 행렬의 행렬식**
$\text{det}(A) = \text{det}(A^T)$
    - 행렬의 전치 행렬의 행렬식은 원래 행렬의 행렬식과 동일함
3. **역행렬의 행렬식**
$\text{det}(A^{-1}) = \frac{1}{\text{det}(A)}$
    - 역행렬의 행렬식은 원래 행렬의 행렬식의 역수임
4. **유사 행렬의 행렬식**
$\text{det}(A) = \text{det}(A')$
    - 유사 행렬 $A, A' (A' = S^{-1}AS)$는 동일한 행렬식을 가짐
5. **삼각 행렬의 행렬식**
$\text{det}(T) = \prod_{i=1}^n T_{ii}$
    - 삼각 행렬(대각선 이외의 원소가 0인 행렬)의 행렬식은 대각선 원소의 곱임 (대각 행렬 포함)
6. **행/열의 상수 추가**
    - 행/열의 상수를 다른 행/열에 더해도 행렬식은 변하지 않음
7. **행/열을 상수 $\lambda$로 곱하기**
$\text{det}(\lambda A) = \lambda^n \cdot \text{det}(A)$
    - 행/열을 상수 $\lambda$로 곱하면 행렬식은 $\lambda^n \cdot \text{det}(A)$로 변함
8. **행/열 교환**
    - 두 행/열을 교환하면 행렬식의 부호가 바뀜

위 (5)-(8)의 성질을 활용하여 가우스 소거법(Gaussian Elimination)으로 행렬을 삼각 행렬로 변환해 행렬식을 계산할 수 있음

### Trace

- 정의
    - 정사각 행렬 $A \in \mathbb{R}^{n \times n}$의 Trace(대각합)는 다음과 같이 정의됨:
        
        $\text{tr}(A) := \sum_{i=1}^n a_{ii}$
        
- $\text{tr}(A + B) = \text{tr}(A) + \text{tr}(B)$
- $\text{tr}(\alpha A) = \alpha \cdot \text{tr}(A)$
- $\text{tr}(I_n) = n$

## 2️⃣ Eigenvalues and Eigenvectors

### Eigenvalue and Eigenvector

- 정의
    - 정사각 행렬  $A \in \mathbb{R}^{n \times n}$ 에 대해:
        - $\lambda \in \mathbb{R}$ 가 행렬 $A$의 **고유값(Eigenvalue)**이고,
        - $x \in \mathbb{R}^n \setminus \{0\}$ 가 $A$에 대응하는 **고유벡터(Eigenvector)**라면, 다음 식이 성립함:
            - $Ax = \lambda x$
- 동등한 조건
    - $\lambda$는 $A$의 고유값이다.
    - $(A - \lambda I_n)x = 0$  방정식이 자명하지 않은 해($x \neq 0$)를 갖는다.
    - $\text{det}(A - \lambda I_n) = 0$
        - 이 조건은 행렬의 고유값을 찾기 위해 사용됨

### Properties

- $A \in \mathbb{R}^{n \times n}$에서 n개의 서로 다른 고유값이 존재하면, 이에 대응하는 고유벡터는 선형 독립적이며 $\mathbb{R}^n$에서 기저를 형성함
    - 반대는 성립하지 않음
    - n개의 고유값보다 적은 고유값으로 n개의 선형 독립적인 고유벡터를 가지는 예시가 존재할 수 있음
- $\text{det}(A) = \prod_{i=1}^n \lambda_i$
- $\text{tr}(A) = \sum_{i=1}^n \lambda_i$

## 3️⃣ Cholesky Decomposition

- 실수: 두 동일한 수의 곱으로 분해 가능, e.g. 9 = 3 x 3
- 정리
    - **대칭(symmetric)**, **양의 정부호(positive definite)** 행렬 $A$에 대해:
        
        $A = LL^T$
        
        - $L$: 대각 원소가 양수인 **하삼각 행렬**
        - 이러한 $L$은 유일하며, 이를 $A$의 **Cholesky factor**라고 부름
    
    <aside>
    💡
    
    양의 정부호(positive definite) 행렬이란, 모든 고유값이 양수인 행렬을 말함
    
    </aside>
    
- 응용
    1. 공분산 행렬의 분해
        1. 다변량 가우시안 변수의 공분산 행렬을 효율적으로 분해
    2. 확률 변수의 선형 변환
        1. 랜덤 변수의 선형 변환에 사용
    3. 행렬식 계산의 효율성 증가
        1. $\text{det}(A) = \text{det}(L) \cdot \text{det}(L^T) = \text{det}(L)^2$
        2. $\text{det}(L) = \prod_i l_{ii}$

## 4️⃣ Eigendecomposition and Diagonalization

### Diagonal Matrix and Diagonalization

- 대각 행렬(Diagonal Matrix)
    - 대각선 이외의 원소가 모두 0인 행렬을 대각 행렬이라고 함
        
        
        $D =\begin{bmatrix}d_1 & 0 & \cdots & 0 \\0 & d_2 & \cdots & 0 \\\vdots & \vdots & \ddots & \vdots \\0 & 0 & \cdots & d_n\end{bmatrix}$
        
    - 대각 행렬의 성질:
        - $D^k =\begin{bmatrix}d_1^k & 0 & \cdots & 0 \\0 & d_2^k & \cdots & 0 \\\vdots & \vdots & \ddots & \vdots \\0 & 0 & \cdots & d_n^k\end{bmatrix}$
        - $D^{-1} =\begin{bmatrix}1/d_1 & 0 & \cdots & 0 \\0 & 1/d_2 & \cdots & 0 \\\vdots & \vdots & \ddots & \vdots \\0 & 0 & \cdots & 1/d_n\end{bmatrix}$
        - 행렬식: $\text{det}(D) = d_1 \cdot d_2 \cdot \cdots \cdot d_n$
- 대각화 가능성(Diagnalizability)
    - 정의: 행렬 $A \in \mathbb{R}^{n \times n}$가 대각화 가능하다는 것은, $A$가 대각 행렬 $D$와 유사하다는 것을 의미함
        - 즉, 가역 행렬 $P \in \mathbb{R}^{n \times n}$가 존재하여:
            
            $D = P^{-1}AP$
            
- 직교 대각화(Orthogonal Diagonalizability)
    - 정의: 행렬 $A \in \mathbb{R}^{n \times n}$가 직교 대각화 가능하다는 것은, $A$가 대각 행렬 $D$와 유사하며, 이때 P가 직교 행렬(orthogonal matrix)임을 의미함
    
    <aside>
    💡
    
    직교 행렬이란, 다음 조건을 만족하는 행렬 $P \in \mathbb{R}^{n \times n}$를 말함
    
    - $P^T P = P P^T = I$
    - 즉, $P^T = P^{-1}$
    </aside>
    

### Power of Diagonalization

- 행렬의 거듭제곱:
    - 행렬 A가 대각화 가능한 경우, 다음과 같이 표현됨:
        
        $A^k = P D^k P^{-1}$
        
- 행렬식 계산:
    - 대각화된 행렬의 행렬식은 다음과 같이 계산할 수 있음:
        
        $\text{det}(A) = \text{det}(P) \cdot \text{det}(D) \cdot \text{det}(P^{-1}) = \text{det}(D)$
        
        대각행렬 $D$의 행렬식은 대각선 원소들의 곱임:
        
        $\text{det}(D) = \prod_i d_{ii}$
        

### Orthogonally Diagonalizable and Symmetric Matrix

- 정리
    - 행렬 $A \in \mathbb{R}^{n \times n}$가 직교 대각화 가능하다. ⇔ $A$가 대칭 행렬이다.
- 질문
    - 직교 대각화 가능할 때, 행렬 $P$(따라서 $D$)를 어떻게 찾을 수 있는가?
- 스펙트럼 정리(Spectral Theorem)
    - 행렬 $A \in \mathbb{R}^{n \times n}$가 대칭 행렬이라면, 다음 성질을 가짐:
        - **고유값은 모두 실수**임
        - **서로 다른 고유값에 대응하는 고유벡터는 서로 수직(직교)**임
        - **직교 고유벡터 집합(Orthogonal Eigenbasis)이 존재**함
- 설명
    - 위 성질에 따라, 행렬 $P$는 $A$의 $n$개의 고유벡터로 구성된 열 벡터를 포함함
    - $AP = PD$이고, $P^T = P^{-1}$(즉, $P$는 직교 행렬)임
    - 이를 통해 $A$를 직교 대각화할 수 있음

## 5️⃣ Singular Value Decomposition

### Storyline

- Eigendecomposition(고유값 분해, EVD)
    - 고유값 분해는 **대칭 행렬** $A \in \mathbb{R}^{n \times n}$에 대한 **(직교) 대각화**를 의미함
- 확장: Singular Value Decomposition(특이값 분해, SVD)
    - 첫 번째 확장: 비대칭이지만 여전히 정사각 행렬인 경우의 대각화 $A \in \mathbb{R}^{n \times n}$
    - 두 번째 확장: 비대칭이며 비정사각 행렬 $A \in \mathbb{R}^{m \times n}$의 대각화
- 배경(Background)
    - 행렬 $A \in \mathbb{R}^{m \times n}$에 대해, $S := A^T A \in \mathbb{R}^{n \times n}$는 항상 다음 성질을 가짐:
        - 대칭성(Symmetric)
            
            $S^T = (A^T A)^T = A^T A = S$
            
        - 양의 준정부호(Positive Semidefinite)
            
            $x^T S x = x^T A^T A x = (Ax)^T (Ax) \geq 0$
            

### Singular Value Decomposition

- 정리
    - 행렬 $A \in \mathbb{R}^{m \times n}$의 랭크가 $r \in [0, \min(m, n)]$일 때, SVD는 다음과 같이 분해됨:
        
        $A = U \Sigma V^T$
        
        - $U$: 직교 행렬 ($U = [u_1 \cdots u_m] \in \mathbb{R}^{m \times m}$)
        - $V$: 직교 행렬 ($V = [v_1 \cdots v_n] \in \mathbb{R}^{n \times n}$)
        - $\Sigma$: $m \times n$ 크기의 대각 행렬로, 대각 원소 $\sigma_i ≥ 0$이고, $i \neq j$일 때는 $\Sigma_{ij} = 0$
- 참고
    - $\Sigma$의 대각 원소 $\sigma_i (i=1,\dots,r)$는 **특이값(Singular Values)**이라고 불림
    - $u_i$와 $v_j$는 각각 **왼쪽 특이벡터(Left Singular Vectors)**와 **오른쪽 특이벡터(Right Singular Vectors)**라고 불림

### SVD: How it Works (for $A \in \mathbb{R}^{n \times n}$)

- 전제 조건
    - 행렬 $A \in \mathbb{R}^{n \times n}$의 랭크가 $r \le n$일 때:
        - $A^TA$는 **대칭 행렬(Symmetric Matrix)**임
- 구성
    1. 랭크 관계
        
        $\text{rk}(A) = \text{rk}(A^T A) = \text{rk}(D) = r$
        
    2. 왼쪽 특이벡터 구성
        
        $u_i = \frac{Av_i}{\sqrt{\lambda_i}}, \quad 1 \leq i \leq r$
        
    3. 직교 기저 확장
        
        $i = r+1, \dots, n$에 대해 $u_i$를 구성하여 $U = (u_1, \dots, u_n)$가 $\mathbb{R}^n$의 직교 기저를 형성하도록 만듬
        
    4. 특이값 행렬 정의
        
        $\Sigma = \text{diag}(\sqrt{\lambda_1}, \dots, \sqrt{\lambda_r})$
        
    5. SVD 확인
        
        $U \Sigma = AV$가 성립
        

### EVD($A = PDP^{-1}$) vs. SVD($A = U\Sigma V^T$)

- 존재 조건
    - SVD: 항상 존재함
    - EVD: **정사각 행렬**에 대해서만 존재하며, 고유벡터의 기저를 찾을 수 있을 때만 가능함 (예: 대칭 행렬)
- 직교성
    - SVD: $U$와 $V$는 항상 직교 행렬이며, 회전을 나타냄
    - EVD: 행렬 $P$는 반드시 직교 행렬이 아님 (대칭 행렬인 경우에만 직교 행렬)
- 도메인(Domain)과 공역(Codomain)의 관계
    - 공통점:
        - 도메인의 기저 변경
        - 새로운 기저 벡터의 독립적인 스케일링
        - 도메인에서 공역으로의 매핑을 포함함
    - 차이점
        - SVD는 도메인과 공역이 다른 벡터 공간일 수 있음
- SVD와 EVD의 관계
    - **SVD와 EVD는 투영(Projection)을 통해 밀접하게 연결**됨:
        - 왼쪽 특이벡터는 $A^TA$의 고유벡터
        - 오른쪽 특이벡터는 $AA^T$의 고유벡터
        - 특이값은 $A^TA$와 $AA^T$의 고유값의 제곱근
    - 특수한 경우:
        - A가 대칭 행렬인 경우, **EVD와 SVD는 동일**함 (스펙트럼 정리에 따라)