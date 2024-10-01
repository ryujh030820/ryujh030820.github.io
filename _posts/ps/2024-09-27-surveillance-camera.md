---
published: true
title: "[프로그래머스] [Level 3] 프로그래머스 단속카메라 (C++)"
author:
  name: JH
  link: https://github.com/ryujh030820
date: 2024-09-27 22:15:00 +0900
categories: [Development, PS]
tags: [ps, 프로그래머스, c++]
---

## 문제 설명

고속도로를 이동하는 모든 차량이 고속도로를 이용하면서 단속용 카메라를 한 번은 만나도록 카메라를 설치하려고 합니다.

고속도로를 이동하는 차량의 경로 routes가 매개변수로 주어질 때, 모든 차량이 한 번은 단속용 카메라를 만나도록 하려면 최소 몇 대의 카메라를 설치해야 하는지를 return 하도록 solution 함수를 완성하세요.

**제한사항**

- 차량의 대수는 1대 이상 10,000대 이하입니다.
- routes에는 차량의 이동 경로가 포함되어 있으며 routes[i][0]에는 i번째 차량이 고속도로에 진입한 지점, routes[i][1]에는 i번째 차량이 고속도로에서 나간 지점이 적혀 있습니다.
- 차량의 진입/진출 지점에 카메라가 설치되어 있어도 카메라를 만난것으로 간주합니다.
- 차량의 진입 지점, 진출 지점은 -30,000 이상 30,000 이하입니다.

## 풀이 1

결국 한번만 카메라를 만나면 되는 것이 핵심! 진출 지점을 기준으로 오름차순으로 정렬한 후, 각 진출 지점마다 카메라를 설치해 check 배열을 통해 검사한다. 이 문제가 이해가 잘 되지 않을 경우, 아래 문제를 풀어보는걸 추천한다. (대표적인 그리디 문제다.)

[14569번: 시간표 짜기](https://www.acmicpc.net/problem/14569)

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

bool check[10000];

bool compare(vector<int> front, vector<int> back) {
    return front[1] < back[1];
}

int solution(vector<vector<int>> routes) {
    int answer = 0;

    sort(routes.begin(), routes.end(), compare); // 진출 지점을 기준으로 정렬

    for(int i = 0; i < routes.size(); i++) {
        int camera = routes[i][1];

        if(!check[i]) {
            answer++;
            check[i] = true;

            for(int j = i + 1; j < routes.size(); j++) {
                if(routes[j][0] <= camera) {
                    check[j] = true;
                }
            }
        }
    }

    return answer;
}
```

## 풀이2

위 풀이로도 정확성 테스트와 효율성 테스트 모두 통과하지만 타 블로그에서 더 좋은 풀이가 있어 소개하도록 하겠다!

(출처 : https://wwlee94.github.io/category/algorithm/greedy/speed-enforcement-camera/)

매번 일일이 각 route들의 진입 지점을 검사하지 않고 routes 배열을 반복하면서 카메라가 진입 지점보다 작은지 확인한다.

만약 카메라가 진입 지점보다 작다면,

1. 카메라를 추가로 세우고
2. 가장 최근 카메라 위치를 route[1]로 갱신한다.

위 풀이를 사용하면 시간 복잡도를 O(n)으로 더 단축할 수 있다.

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

bool compare(vector<int> front, vector<int> back) {
    return front[1] < back[1];
}

int solution(vector<vector<int>> routes) {
    int answer = 0;
    int camera = -30001;

    sort(routes.begin(), routes.end(), compare); // 진출 지점을 기준으로 정렬

    for(int i = 0; i < routes.size(); i++) {
        if(camera < routes[i][0]) {
            answer++;
            camera = routes[i][1];
        }
    }

    return answer;
}
```
