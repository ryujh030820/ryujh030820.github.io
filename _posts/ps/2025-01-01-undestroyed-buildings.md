---
published: true
title: "[프로그래머스] [Level 3] 파괴되지 않은 건물 (C++)"
date: 2025-01-01 20:21:00 +0900
categories: [Development, PS]
tags: [ps, 프로그래머스, c++]
---
## 문제 설명

N x M 크기의 행렬 모양의 게임 맵이 있습니다. 이 맵에는 내구도를 가진 건물이 각 칸마다 하나씩 있습니다. 적은 이 건물들을 공격하여 파괴하려고 합니다. 건물은 적의 공격을 받으면 내구도가 감소하고 내구도가 0이하가 되면 파괴됩니다. 반대로, 아군은 회복 스킬을 사용하여 건물들의 내구도를 높이려고 합니다.

적의 공격과 아군의 회복 스킬은 항상 직사각형 모양입니다.

예를 들어, 아래 사진은 크기가 4 x 5인 맵에 내구도가 5인 건물들이 있는 상태입니다.

![1.png](/assets/img/undestroyed-buildings/1.png)

첫 번째로 적이 맵의 **(0,0)부터 (3,4)까지 공격하여 4만큼** 건물의 내구도를 낮추면 아래와 같은 상태가 됩니다.

![2.png](/assets/img/undestroyed-buildings/2.png)

두 번째로 적이 맵의 **(2,0)부터 (2,3)까지 공격하여 2만큼** 건물의 내구도를 낮추면 아래와 같이 4개의 건물이 파괴되는 상태가 됩니다.

![3.png](/assets/img/undestroyed-buildings/3.png)

세 번째로 아군이 맵의 **(1,0)부터 (3,1)까지 회복하여 2만큼** 건물의 내구도를 높이면 아래와 같이 **2개의 건물이 파괴되었다가 복구**되고 2개의 건물만 파괴되어있는 상태가 됩니다.

![4.png](/assets/img/undestroyed-buildings/4.png)

마지막으로 적이 맵의 **(0,1)부터 (3,3)까지 공격하여 1만큼** 건물의 내구도를 낮추면 아래와 같이 8개의 건물이 더 파괴되어 총 10개의 건물이 파괴된 상태가 됩니다. **(내구도가 0 이하가 된 이미 파괴된 건물도, 공격을 받으면 계속해서 내구도가 하락하는 것에 유의해주세요.)**

![5.png](/assets/img/undestroyed-buildings/5.png)

최종적으로 총 10개의 건물이 파괴되지 않았습니다.

건물의 내구도를 나타내는 2차원 정수 배열 `board`와 적의 공격 혹은 아군의 회복 스킬을 나타내는 2차원 정수 배열 `skill`이 매개변수로 주어집니다. 적의 공격 혹은 아군의 회복 스킬이 모두 끝난 뒤 파괴되지 않은 건물의 개수를 return하는 solution함수를 완성해 주세요.

## 풀이

해당 문제를 딱 보고 드는 생각은 ‘그냥 3중 for문 써서 풀면 되는거 아닌가..?’겠지만 3중 for문으로 코드 제출을 할 시, 이를 비웃듯 효율성 테스트에서 시간 초과가 뜬다.

정확도 테스트에서는 `board`의 크기가 최대 100x100이기 때문에, 100^3 = 1000000으로 3중 for문을 써도 별 문제가 없지만 효율성 테스트에서는 배수의 크기를 사용하기 때문에 3중 for문보다 효율적인 풀이가 필요하다.

결론적으로 해당 문제는 **누적합**을 이용해 푸는 문제이다.

그 중에서도 2차원 배열 누적합을 사용한다. 자세한건 아래 글을 참고하면 된다.

<a href="https://yabmoons.tistory.com/728">https://yabmoons.tistory.com/728</a>

```cpp
#include <string>
#include <vector>

using namespace std;

int accSum[1010][1010];

int solution(vector<vector<int>> board, vector<vector<int>> skill) {
    int answer = 0;
    
    int N = board.size();
    int M = board[0].size();
    
    for(int i = 0; i < skill.size(); i++) {
        int type = skill[i][0];
        int r1 = skill[i][1];
        int c1 = skill[i][2];
        int r2 = skill[i][3];
        int c2 = skill[i][4];
        int degree = skill[i][5];
        
        if(type == 1) {
            degree *= -1;
        }
        
        accSum[r1][c1] += degree;
        accSum[r2 + 1][c1] += -degree;
        accSum[r1][c2 + 1] += -degree;
        accSum[r2 + 1][c2 + 1] += degree;
    }
    
    // 행 누적합
    for(int i = 0; i < N; i++) {
        for(int j = 1; j < M; j++) {
            accSum[i][j] += accSum[i][j - 1];
        }
    }
    
    // 열 누적합
    for(int j = 0; j < M; j++) {
        for(int i = 1; i < N; i++) {
            accSum[i][j] += accSum[i - 1][j];
        }
    }
    
    // board와 더하기
    for(int i = 0; i < N; i++) {
        for(int j = 0; j < M; j++) {
            board[i][j] += accSum[i][j];
            
            if(board[i][j] > 0) {
                answer++;
            }
        }
    }
    
    return answer;
}
```