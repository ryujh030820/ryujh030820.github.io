---
published: true
title: "[프로그래머스] [Level 3] 경주로 건설 (C++)"
author:
  name: JH
  link: https://github.com/ryujh030820
date: 2024-09-27 22:23:00 +0900
categories: [Development, PS]
tags: [ps, 프로그래머스, c++]
---

{% raw %}

## 문제 설명

건설회사의 설계사인 `죠르디`는 고객사로부터 자동차 경주로 건설에 필요한 견적을 의뢰받았습니다.

제공된 경주로 설계 도면에 따르면 경주로 부지는 `N x N` 크기의 정사각형 격자 형태이며 각 격자는 `1 x 1` 크기입니다.

설계 도면에는 각 격자의 칸은 `0` 또는 `1` 로 채워져 있으며, `0`은 칸이 비어 있음을 `1`은 해당 칸이 벽으로 채워져 있음을 나타냅니다.

경주로의 출발점은 (0, 0) 칸(좌측 상단)이며, 도착점은 (N-1, N-1) 칸(우측 하단)입니다. 죠르디는 출발점인 (0, 0) 칸에서 출발한 자동차가 도착점인 (N-1, N-1) 칸까지 무사히 도달할 수 있게 중간에 끊기지 않도록 경주로를 건설해야 합니다.

경주로는 상, 하, 좌, 우로 인접한 두 빈 칸을 연결하여 건설할 수 있으며, 벽이 있는 칸에는 경주로를 건설할 수 없습니다.

이때, 인접한 두 빈 칸을 상하 또는 좌우로 연결한 경주로를 `직선 도로` 라고 합니다.

또한 두 `직선 도로`가 서로 직각으로 만나는 지점을 `코너` 라고 부릅니다.

건설 비용을 계산해 보니 `직선 도로` 하나를 만들 때는 100원이 소요되며, `코너`를 하나 만들 때는 500원이 **추가**로 듭니다.

죠르디는 견적서 작성을 위해 경주로를 건설하는 데 필요한 최소 비용을 계산해야 합니다.

예를 들어, 아래 그림은 `직선 도로` 6개와 `코너` 4개로 구성된 임의의 경주로 예시이며, 건설 비용은 6 x 100 + 4 x 500 = 2600원 입니다.

![kakao_road2.png](/assets/img/kakao_road2.png)

또 다른 예로, 아래 그림은 `직선 도로` 4개와 `코너` 1개로 구성된 경주로이며, 건설 비용은 4 x 100 + 1 x 500 = 900원 입니다.

![kakao_road3.png](/assets/img/kakao_road3.png)

도면의 상태(0은 비어 있음, 1은 벽)을 나타내는 2차원 배열 board가 매개변수로 주어질 때, 경주로를 건설하는데 필요한 최소 비용을 return 하도록 solution 함수를 완성해주세요.

### **[제한사항]**

- board는 2차원 정사각 배열로 배열의 크기는 3 이상 25 이하입니다.
- board 배열의 각 원소의 값은 0 또는 1 입니다.
  - 도면의 가장 왼쪽 상단 좌표는 (0, 0)이며, 가장 우측 하단 좌표는 (N-1, N-1) 입니다.
  - 원소의 값 0은 칸이 비어 있어 도로 연결이 가능함을 1은 칸이 벽으로 채워져 있어 도로 연결이 불가능함을 나타냅니다.
- board는 항상 출발점에서 도착점까지 경주로를 건설할 수 있는 형태로 주어집니다.
- 출발점과 도착점 칸의 원소의 값은 항상 0으로 주어집니다.

---

## 풀이

**DFS** 혹은 **BFS**로 풀 수 있는 문제다. 최소 비용을 구해야 하므로 일반적인 큐 대신 우선순위 큐를 사용하였다.

방향이 바뀌지 않는 경우에는 기존 비용에 100을 더해주었고, 방향이 바뀌는 경우에는 500을 추가로, 즉 600을 더해주었다.

처음에 이 **추가로 든다**는 말을 발견 못하고 500씩만 더해줬다가 계속 오차가 생겨서 엄한 짓 했다(…).

사실 쉬울줄 알았는데 생각보다 삽질 좀 했다.

```cpp
#include <string>
#include <vector>
#include <queue>

using namespace std;

int dx[] = {1, 0, -1, 0};
int dy[] = {0, 1, 0, -1};
bool visited[25][25][4];

int solution(vector<vector<int>> board) {
    int answer = 0;
    priority_queue<pair<pair<int, int>, pair<int, int>>> q; // 비용, 방향, row, col 순, 최소 비용을 지닌 도로가 아닌 다른 도로가 선택될 수 있으므로 힙 자료구조 사용

    q.push({{0, 0}, {0, 0}});
    q.push({{0, 1}, {0, 0}});
    q.push({{0, 2}, {0, 0}});
    q.push({{0, 3}, {0, 0}});

    while(!q.empty()) {
        int expense = -q.top().first.first;
        int direction = q.top().first.second;
        int row = q.top().second.first;
        int col = q.top().second.second;
        q.pop();

        if(visited[row][col][direction]) {
            continue;
        }

        visited[row][col][direction] = true;

        if(row == board.size() - 1 && col == board.size() - 1) {
            answer = expense;
            break;
        }

        for(int i = 0; i < 4; i++) {
            int nextRow = row + dy[i];
            int nextCol = col + dx[i];

            if(nextRow >= 0 && nextCol >= 0 && nextRow < board.size() && nextCol < board.size() && board[nextRow][nextCol] == 0) {
                int newExpense;

                if(i == direction) {
                    newExpense = expense + 100;
                } else {
                    newExpense = expense + 600;
                }

                q.push({{-newExpense, i}, {nextRow, nextCol}});
            }
        }
    }

    return answer;
}
```

{% endraw %}
