---
published: true
title: "[프로그래머스] [Level 3] 여행경로 (C++)"
author:
  name: JH
  link: https://github.com/ryujh030820
date: 2024-09-27 22:22:00 +0900
categories: [Development, PS]
tags: [ps, 프로그래머스, c++]
---

## 문제 설명

> 주어진 항공권을 모두 이용하여 여행경로를 짜려고 합니다. 항상 "ICN" 공항에서 출발합니다.
>
> 항공권 정보가 담긴 2차원 배열 tickets가 매개변수로 주어질 때, 방문하는 공항 경로를 배열에 담아 return 하도록 solution 함수를 작성해주세요.
>
> ### 제한사항
>
> - 모든 공항은 알파벳 대문자 3글자로 이루어집니다.
> - 주어진 공항 수는 3개 이상 10,000개 이하입니다.
> - tickets의 각 행 [a, b]는 a 공항에서 b 공항으로 가는 항공권이 있다는 의미입니다.
> - 주어진 항공권은 모두 사용해야 합니다.
> - 만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return 합니다.
> - 모든 도시를 방문할 수 없는 경우는 주어지지 않습니다.

## 입출력 예

| tickets                                                                         | return                                     |
| ------------------------------------------------------------------------------- | ------------------------------------------ |
| [["ICN", "JFK"], ["HND", "IAD"], ["JFK", "HND"]]                                | ["ICN", "JFK", "HND", "IAD"]               |
| [["ICN", "SFO"], ["ICN", "ATL"], ["SFO", "ATL"], ["ATL", "ICN"], ["ATL","SFO"]] | ["ICN", "ATL", "ICN", "SFO", "ATL", "SFO"] |

## 풀이

대표적인 DFS/BFS 문제이다. 나같은 경우, 최소 경로의 길이가 정해져 있지 않을 때 최소 경로를 구하는 문제를 제외한 문제들은 DFS를 선호하기 때문에 DFS로 풀었다. BFS로 풀어도 상관없다.

**만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return**하라는 조건이 있기 때문에 ticket를 sort 함수를 이용해 정렬해주고, 특정 공항에서 출발하는 모든 항공권들을 탐색하는데 만약 해당 항공권을 사용해 계속 탐색을 진행했을 시 모든 항공권을 사용하지 못할 경우에는 정답인 route를 pop해 백트래킹을 수행하였다.

정답 코드는 아래와 같다.

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>

using namespace std;

vector<string> route;
bool isFind = false;
bool isUsed[1000000];

void DFS(string airport, vector<vector<string>> tickets, vector<string>& answer) {
    if(route.size() == tickets.size() + 1) {
        isFind = true;
        answer = route;
        return;
    }

    for(int i = 0; i < tickets.size(); i++) {
        if(tickets[i][0] == airport) {
            if(!isUsed[i]) {
                isUsed[i] = true;
                route.push_back(tickets[i][1]);
                DFS(tickets[i][1], tickets, answer);

                if(!isFind) {
                    isUsed[i] = false;
                    route.pop_back();
                }
            }
        }
    }
}

vector<string> solution(vector<vector<string>> tickets) {
    vector<string> answer;

    sort(tickets.begin(), tickets.end());

    for(int i = 0; i < tickets.size(); i++) {
        cout << tickets[i][0] << "->" << tickets[i][1] << endl;
    }

    route.push_back("ICN");

    DFS("ICN", tickets, answer);

    return answer;
}
```
