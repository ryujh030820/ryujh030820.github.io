---
published: true
title: "[프로그래머스] [Level 3] 보석 쇼핑 (C++)"
author:
  name: JH
  link: https://github.com/ryujh030820
date: 2024-09-27 22:20:00 +0900
categories: [Development, PS]
tags: [ps, 프로그래머스, c++]
---

## 문제 설명

> 개발자 출신으로 세계 최고의 갑부가 된 `어피치`는 스트레스를 받을 때면 이를 풀기 위해 오프라인 매장에 쇼핑을 하러 가곤 합니다.
>
> 어피치는 쇼핑을 할 때면 매장 진열대의 특정 범위의 물건들을 모두 싹쓸이 구매하는 습관이 있습니다.
>
> 어느 날 스트레스를 풀기 위해 보석 매장에 쇼핑을 하러 간 어피치는 이전처럼 진열대의 특정 범위의 보석을 모두 구매하되 특별히 아래 목적을 달성하고 싶었습니다.
>
> `진열된 모든 종류의 보석을 적어도 1개 이상 포함하는 가장 짧은 구간을 찾아서 구매`
>
> 예를 들어 아래 진열대는 4종류의 보석(RUBY, DIA, EMERALD, SAPPHIRE) 8개가 진열된 예시입니다.
>
> | 진열대 번호 | 1   | 2    | 3        | 4       | 5       | 6           | 7            | 8   |
> | ----------- | --- | ---- | -------- | ------- | ------- | ----------- | ------------ | --- |
> | 보석 이름   | DIA | RUBY | **RUBY** | **DIA** | **DIA** | **EMERALD** | **SAPPHIRE** | DIA |
>
> 진열대의 3번부터 7번까지 5개의 보석을 구매하면 모든 종류의 보석을 적어도 하나 이상씩 포함하게 됩니다.
>
> 진열대의 3, 4, 6, 7번의 보석만 구매하는 것은 중간에 특정 구간(5번)이 빠지게 되므로 어피치의 쇼핑 습관에 맞지 않습니다.
>
> 진열대 번호 순서대로 보석들의 이름이 저장된 배열 gems가 매개변수로 주어집니다. 이때 모든 보석을 하나 이상 포함하는 가장 짧은 구간을 찾아서 return 하도록 solution 함수를 완성해주세요.
>
> 가장 짧은 구간의 `시작 진열대 번호`와 `끝 진열대 번호`를 차례대로 배열에 담아서 return 하도록 하며, 만약 가장 짧은 구간이 여러 개라면 `시작 진열대 번호`가 가장 작은 구간을 return 합니다.

## 풀이

처음에는 무식하게 set을 통해 보석의 수를 구하고, map을 이용해 각 보석의 구매 여부를 확인하여 for문을 이용해 모든 시작 진열대 번호마다 가장 짧은 구간을 구하는 방식으로 코드를 짰으나 당연히 효율성 테스트에서 fail이 났다.

**구간**을 구하는 문제라면 당연히 **투 포인터 알고리즘**을 떠올렸어야 한다.

또한, 처음에는 그냥 map을 사용했으나 굳이 정렬이 필요없는 경우에는 **unordered_map**이 더 효율적이라고 한다. 테스트해본 결과, 일반 map을 써도 효율성 테스트를 통과하기는 한다.

```cpp
#include <string>
#include <vector>
#include <unordered_map>
#include <set>

using namespace std;

unordered_map<string, int> gemList; // map 정렬 필요 없으므로
set<string> kind;

vector<int> solution(vector<string> gems) {
    // 투포인터 알고리즘, 보석 개수 계산해가며 구간 좁히기

    vector<int> answer(2);

    int start = 0;
    int end = 0;
    int min = 100001;

    for(int i = 0; i < gems.size(); i++) {
        kind.insert(gems[i]);
    }

    while(1) {
        for(int i = end; i < gems.size(); i++) { // 구간이 모든 보석을 포함하도록 end를 증가
            gemList[gems[i]]++;

            if(gemList.size() == kind.size()) {
                end = i;
                break;
            }

            if(i == gems.size() - 1) {
                return answer;
            }
        }

        while(gemList[gems[start]] > 1) { // start를 증가시키면서 구간 좁히기
            gemList[gems[start]]--;
            start++;
        }

        if(end - start < min) {
            min = end - start; // end - start가 min보다 작으면 min과 answer 업데이트
            answer[0] = start + 1;
            answer[1] = end + 1;
        }

        gemList.erase(gems[start]);
        start++;
        end++; // 새로운 구간합을 구하기 위해 start와 end 추가

        if(end >= gems.size()) {
            break;
        }
    }

    return answer;
}
```
