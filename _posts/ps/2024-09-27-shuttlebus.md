---
published: true
title: "[프로그래머스] [Level 3] 셔틀버스 (C++)"
date: 2024-09-27 22:34:00 +0900
categories: [Development, PS]
tags: [ps, 프로그래머스, c++]
---

## 문제 설명

카카오에서는 무료 셔틀버스를 운행하기 때문에 판교역에서 편하게 사무실로 올 수 있다. 카카오의 직원은 서로를 '크루'라고 부르는데, 아침마다 많은 크루들이 이 셔틀을 이용하여 출근한다.

이 문제에서는 편의를 위해 셔틀은 다음과 같은 규칙으로 운행한다고 가정하자.

- 셔틀은 `09:00`부터 총 `n`회 `t`분 간격으로 역에 도착하며, 하나의 셔틀에는 최대 `m`명의 승객이 탈 수 있다.
- 셔틀은 도착했을 때 도착한 순간에 대기열에 선 크루까지 포함해서 대기 순서대로 태우고 바로 출발한다. 예를 들어 `09:00`에 도착한 셔틀은 자리가 있다면 `09:00`에 줄을 선 크루도 탈 수 있다.

일찍 나와서 셔틀을 기다리는 것이 귀찮았던 콘은, 일주일간의 집요한 관찰 끝에 어떤 크루가 몇 시에 셔틀 대기열에 도착하는지 알아냈다. 콘이 셔틀을 타고 사무실로 갈 수 있는 도착 시각 중 제일 늦은 시각을 구하여라.

단, 콘은 게으르기 때문에 같은 시각에 도착한 크루 중 대기열에서 제일 뒤에 선다. 또한, 모든 크루는 잠을 자야 하므로 `23:59`에 집에 돌아간다. 따라서 어떤 크루도 다음날 셔틀을 타는 일은 없다.

## 입력 형식

셔틀 운행 횟수 `n`, 셔틀 운행 간격 `t`, 한 셔틀에 탈 수 있는 최대 크루 수 `m`, 크루가 대기열에 도착하는 시각을 모은 배열 `timetable`이 입력으로 주어진다.

- 0 ＜  `n` ≦ 10
- 0 ＜  `t` ≦ 60
- 0 ＜  `m` ≦ 45
- `timetable`은 최소 길이 1이고 최대 길이 2000인 배열로, 하루 동안 크루가 대기열에 도착하는 시각이 `HH:MM` 형식으로 이루어져 있다.
- 크루의 도착 시각 `HH:MM`은 `00:01`에서 `23:59` 사이이다.

## 출력 형식

콘이 무사히 셔틀을 타고 사무실로 갈 수 있는 제일 늦은 도착 시각을 출력한다. 도착 시각은 `HH:MM` 형식이며, `00:00`에서 `23:59` 사이의 값이 될 수 있다.

## 입출력 예제

| n   | t   | m   | timetable                                                                                                                                       | answer  |
| --- | --- | --- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| 1   | 1   | 5   | ["08:00", "08:01", "08:02", "08:03"]                                                                                                            | "09:00" |
| 2   | 10  | 2   | ["09:10", "09:09", "08:00"]                                                                                                                     | "09:09" |
| 2   | 1   | 2   | ["09:00", "09:00", "09:00", "09:00"]                                                                                                            | "08:59" |
| 1   | 1   | 5   | ["00:01", "00:01", "00:01", "00:01", "00:01"]                                                                                                   | "00:00" |
| 1   | 1   | 1   | ["23:59"]                                                                                                                                       | "09:00" |
| 10  | 60  | 45  | ["23:59","23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59"] | "18:00" |

## 풀이

우선 timetable에 존재하는 크루들을 버스에 배정한 후, 마지막 버스의 인원에 따라 콘의 도착 시간을 결정한다. 나중에 다른 사람들의 풀이를 찾아보니 이분 탐색으로 풀면 좀 더 시간 복잡도를 단축할 수 있는 것 같다.

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <string>
#include <iostream>

using namespace std;

string convertTime(int time) {
    string answer = "";

    int hour = time / 60;
    int minute = time % 60;

    if(hour < 10) {
        answer += "0";
    }

    answer += to_string(hour);
    answer += ":";

    if(minute < 10) {
        answer += "0";
    }

    answer += to_string(minute);

    return answer;
}

string solution(int n, int t, int m, vector<string> timetable) {
    string answer = "";
    int answerNum = 0;
    vector<int> timeTable;
    vector<vector<int>> bus(10);
    int currentTime = 540, busCount = 0, startIdx = 0; // 09:00 -> 00:00부터 540분이 지남

    sort(timetable.begin(), timetable.end());

    for(int i = 0; i < timetable.size(); i++) {
        timeTable.push_back(stoi(timetable[i].substr(0, 2)) * 60 + stoi(timetable[i].substr(3, 2)));
    }

    // 모든 timetable에 존재하는 크루를 버스에 배정
    while(busCount < n) {
        int passengerCount = 0;

        busCount++;

        for(int i = startIdx; i < timeTable.size(); i++) {
            if(timeTable[i] <= currentTime && passengerCount < m) {
                bus[busCount - 1].push_back(timeTable[i]);
                passengerCount++;
                startIdx = i + 1;
            } else {
                break;
            }
        }

        currentTime += t;
    }

    // 마지막 버스에 인원이 가득 차있으면, 마지막으로 탄 크루의 시간에서 1을 뺌
    if(bus[busCount - 1].size() == m) {
        answerNum = bus[busCount - 1][m - 1] - 1;
    } else { // 마지막 버스에 인원이 가득 차지 않았을 시, 셔틀이 도착한 시간
        answerNum = currentTime - t;
    }

    answer = convertTime(answerNum);

    return answer;
}
```
