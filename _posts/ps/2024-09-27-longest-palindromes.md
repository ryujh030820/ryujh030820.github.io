---
published: true
title: "[프로그래머스] [Level 3] 가장 긴 팰린드롬 (C++)"
author:
  name: JH
  link: https://github.com/ryujh030820
date: 2024-09-27 22:33:00 +0900
categories: [Development, PS]
tags: [ps, 프로그래머스, c++]
---

## 문제 설명

앞뒤를 뒤집어도 똑같은 문자열을 팰린드롬(palindrome)이라고 합니다.
문자열 s가 주어질 때, s의 부분문자열(Substring)중 가장 긴 팰린드롬의 길이를 return 하는 solution 함수를 완성해 주세요.

예를들면, 문자열 s가 "abcdcba"이면 7을 return하고 "abacde"이면 3을 return합니다.

## 제한 사항

- 문자열 s의 길이 : 2,500 이하의 자연수
- 문자열 s는 알파벳 소문자로만 구성

## 입출력 예

| s         | answer |
| --------- | ------ |
| "abcdcba" | 7      |
| "abacde"  | 3      |

## 입출력 예 설명

입출력 예 #1

4번째자리 'd'를 기준으로 문자열 s 전체가 팰린드롬이 되므로 7을 return합니다.

입출력 예 #2

2번째자리 'b'를 기준으로 "aba"가 팰린드롬이 되므로 3을 return합니다.

## 풀이 1

```cpp
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

int palindrome(string s, int left, int right) {
    while(left >= 0 && right < s.size() && s[left] == s[right]) {
        left--;
        right++;
    }

    return right - left - 1;
}

int solution(string s)
{
    int answer=1;

    for(int i = s.size() - 1; i >= 0; i--) {
        answer = max(answer, palindrome(s, i, i)); // 홀수인 경우
        answer = max(answer, palindrome(s, i, i + 1)); // 짝수인 경우
    }

    return answer;
}
```

팰린드롬 문자열의 길이가 홀수인 경우와 짝수인 경우로 나누어 모든 중간 지점을 기준으로 탐색할 시, 시간 복잡도가 $O(n^2)$이 나오므로 문제 없이 통과한다.

사실 실전에서는 이게 제일 무난한 풀이인 것 같다.

## 풀이 2 (DP)

```cpp
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

int DP[2500][2500];

int solution(string s)
{
    int answer=1;

    // 1자리
    for(int i = 0; i < s.size(); i++) {
        DP[i][i] = 1;
    }

    // 2자리
    for(int i = 0; i < s.size() - 1; i++) {
        if(s[i] == s[i + 1]) {
            DP[i][i + 1] = 1;
            answer = 2;
        }
    }

    // 3자리 이상
    for(int i = 3; i <= s.size(); i++) { // 팰린드롬 문자열의 길이
        for(int j = 0; j <= s.size() - i; j++) {
            if(s[j] == s[j + i - 1] && DP[j + 1][j + i - 2] == 1) {
                DP[j][j + i - 1] = 1;
                answer = max(answer, i);
            }
        }
    }

    return answer;
}
```

위 코드와 같이 팰린드롬 문자열의 길이를 기준으로 DP[i][j] = i부터 j까지의 팰린드롬 문자열의 여부로 둬 점화식을 세울 수 있다.

시간 복잡도는 마찬가지로 $O(n^2)$이 나온다.
