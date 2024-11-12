---
published: true
title: "[프로그래머스] [Level 3] 자물쇠와 열쇠 (C++)"
date: 2024-11-12 15:40:00 +0900
categories: [Development, PS]
tags: [ps, 프로그래머스, c++]
---
## 문제 설명

고고학자인  **"튜브"**는 고대 유적지에서 보물과 유적이 가득할 것으로 추정되는 비밀의 문을 발견하였습니다. 그런데 문을 열려고 살펴보니 특이한 형태의 **자물쇠**로 잠겨 있었고 문 앞에는 특이한 형태의 **열쇠**와 함께 자물쇠를 푸는 방법에 대해 다음과 같이 설명해 주는 종이가 발견되었습니다.

잠겨있는 자물쇠는 격자 한 칸의 크기가  **`1 x 1`**인  **`N x N`** 크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 **`M x M`** 크기인 정사각 격자 형태로 되어 있습니다.

자물쇠에는 홈이 파여 있고 열쇠 또한 홈과 돌기 부분이 있습니다. 열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠의
 홈 부분에 딱 맞게 채우면 자물쇠가 열리게 되는 구조입니다. 자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는
 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 
자물쇠의 돌기가 만나서는 안됩니다. 또한 자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.

열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return 하도록 solution 함수를 완성해주세요.

## 제한사항

- key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.
- lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.
- M은 항상 N 이하입니다.
- key와 lock의 원소는 0 또는 1로 이루어져 있습니다.
    - 0은 홈 부분, 1은 돌기 부분을 나타냅니다.

## 입출력 예

| key | lock | result |
| --- | --- | --- |
| [[0, 0, 0], [1, 0, 0], [0, 1, 1]] | [[1, 1, 1], [1, 1, 0], [1, 0, 1]] | true |

## 풀이

이 문제에서 핵심은 **자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안된다**는 문제의 설명이라고 생각한다.

즉, key가 일부분 lock을 벗어나도 된다는 말인데 입출력 예시만 보더라도 key가 lock을 벗어난 것을 볼 수 있다.

![1.png](/assets/img/lock-and-key/1.png)

이를 어떻게 처리해줄 것이냐가 중요한데, 우선 key가 한 칸이라도 겹친다면 고려를 해줘야 한다.

따라서 이를 위해, `2*(M-1)+N x 2*(M-1)+N`  크기의 새로운 lock을 선언해주었다. 

![이런 식으로…](/assets/img/lock-and-key/2.png)

이런 식으로…

그 다음, 좌측 상단에서부터 key를 슬라이딩 해주며 key와 lock을 더해준 후, lock의 정중앙 부분(즉, 원래 lock의 부분만 고려)이 모두 1이면 true, 아니면 false를 반환한다.

여담으로, 오른쪽으로 90도 회전하는 코드 같은 경우 요긴하게 많이 쓰니 외워두도록 하자.

```cpp
#include <string>
#include <vector>

using namespace std;

int M, N, B;

// 두 행렬을 더해서 lock이 모두 1이 될 시 true, 아니면 false
bool check(vector<vector<int>> newLock, vector<vector<int>> key, int row, int col) {
    for(int i = 0; i < M; i++) {
        for(int j = 0; j < M; j++) {
            newLock[row + i][col + j] += key[i][j];
        }
    }
    
    for(int i = 0; i < N; i++) {
        for(int j = 0; j < N; j++) {
            if(newLock[i + M - 1][j + M - 1] != 1) {
                return false;
            }
        }
    }
    
    return true;
}

// 오른쪽으로 90도 회전
void rotation(vector<vector<int>>& key) {
    vector<vector<int>> newKey(M, vector<int>(M, 0));
    
    for(int i = 0; i < M; i++) {
        for(int j = 0; j < M; j++) {
            newKey[i][j] = key[M - (j + 1)][i];
        }
    }
    
    key = newKey;
}

bool solution(vector<vector<int>> key, vector<vector<int>> lock) {
    bool answer = true;
    
    M = key.size();
    N = lock.size();
    B = 2 * (M - 1) + N;
    
    vector<vector<int>> newLock(B, vector<int>(B, 0));
    
    // newLock의 중앙에 lock이 위치
    for(int i = 0; i < N; i++) {
        for(int j = 0; j < N; j++) {
            newLock[i + M - 1][j + M - 1] = lock[i][j];
        }
    }
    
    for(int i = 0; i < 4; i++) {
        rotation(key);
        
        for(int j = 0; j <= B - M; j++) {
            for(int k = 0; k <= B - M; k++) {
                if(check(newLock, key, j, k)) {
                    return answer;
                }
            }
        }
    }
    
    answer = false;
    
    return answer;
}
```