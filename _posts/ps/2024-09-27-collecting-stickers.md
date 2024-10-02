---
published: true
title: "[프로그래머스] [Level 3] 프로그래머스 스티커 모으기(2) (C++)"
date: 2024-09-27 22:18:00 +0900
categories: [Development, PS]
tags: [ps, 프로그래머스, c++]
---

## 문제 설명

> 원형으로 연결된 스티커에서 몇 장의 스티커를 뜯어내어 뜯어낸 스티커에 적힌 숫자의 합이 최대가 되도록 하고 싶습니다. 단 스티커 한 장을 뜯어내면 양쪽으로 인접해있는 스티커는 찢어져서 사용할 수 없게 됩니다.
>
> 예를 들어 위 그림에서 14가 적힌 스티커를 뜯으면 인접해있는 10, 6이 적힌 스티커는 사용할 수 없습니다. 스티커에 적힌 숫자가 배열 형태로 주어질 때, 스티커를 뜯어내어 얻을 수 있는 숫자의 합의 최댓값을 return 하는 solution 함수를 완성해 주세요. 원형의 스티커 모양을 위해 배열의 첫 번째 원소와 마지막 원소가 서로 연결되어 있다고 간주합니다.

## 풀이

처음에는 DFS를 생각했으나 깊이가 50000이 넘어서 포기. 일차원 DP로 푸는 문제다.

1. DP[i - 2] + sticker[i] (스티커를 뗀 경우)
2. DP[i - 1] (스티커를 떼지 않은 경우)

로 나눠 점화식을 세운 뒤, 첫번째 스티커를 뗀 경우와 떼지 않은 경우만 별도로 잘 처리해주면 된다. 추가로 테스트 케이스 중 스티커 배열의 길이가 1인 경우가 있어 역시 유의해 풀어야 한다. DP로 푸는 발상만 잘 떠올리면 어렵지 않은 문제.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int DP1[100000]; // 첫번째 스티커를 뗀 경우
int DP2[100000]; // 첫번째 스티커를 떼지 않은 경우

int solution(vector<int> sticker)
{
    int answer = 0;

    if(sticker.size() == 1) {
        answer = sticker[0];
        return answer;
    }

    DP1[0] = sticker[0];
    DP1[1] = sticker[0];

    for(int i = 2; i < sticker.size() - 1; i++) {
        DP1[i] = max(DP1[i - 2] + sticker[i], DP1[i - 1]);
    }

    DP1[sticker.size() - 1] = DP1[sticker.size() - 2];

    DP2[0] = 0;
    DP2[1] = sticker[1];

    for(int i = 2; i < sticker.size(); i++) {
        DP2[i] = max(DP2[i - 2] + sticker[i], DP2[i - 1]);
    }

    answer = max(DP1[sticker.size() - 1], DP2[sticker.size() - 1]);

    return answer;
}
```
