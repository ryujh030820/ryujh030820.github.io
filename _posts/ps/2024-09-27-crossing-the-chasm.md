---
published: true
title: "[프로그래머스] [Level 3] 징검다리 건너기 (C++)"
author:
  name: JH
  link: https://github.com/ryujh030820
date: 2024-09-27 22:21:00 +0900
categories: [Development, PS]
tags: [ps, 프로그래머스, c++]
---

## 문제 설명

> **[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]**
>
> 카카오 초등학교의 "니니즈 친구들"이 "라이언" 선생님과 함께 가을 소풍을 가는 중에 **징검다리**가 있는 개울을 만나서 건너편으로 건너려고 합니다. "라이언" 선생님은 "니니즈 친구들"이 무사히 징검다리를 건널 수 있도록 다음과 같이 규칙을 만들었습니다.
>
> - 징검다리는 일렬로 놓여 있고 각 징검다리의 디딤돌에는 모두 숫자가 적혀 있으며 디딤돌의 숫자는 한 번 밟을 때마다 1씩 줄어듭니다.
> - 디딤돌의 숫자가 0이 되면 더 이상 밟을 수 없으며 이때는 그 다음 디딤돌로 한번에 여러 칸을 건너 뛸 수 있습니다.
> - 단, 다음으로 밟을 수 있는 디딤돌이 여러 개인 경우 무조건 가장 가까운 디딤돌로만 건너뛸 수 있습니다.
>
> "니니즈 친구들"은 개울의 왼쪽에 있으며, 개울의 오른쪽 건너편에 도착해야 징검다리를 건넌 것으로 인정합니다.
>
> "니니즈 친구들"은 한 번에 한 명씩 징검다리를 건너야 하며, 한 친구가 징검다리를 모두 건넌 후에 그 다음 친구가 건너기 시작합니다.
>
> 디딤돌에 적힌 숫자가 순서대로 담긴 배열 stones와 한 번에 건너뛸 수 있는 디딤돌의 최대 칸수 k가 매개변수로 주어질 때, 최대 몇 명까지 징검다리를 건널 수 있는지 return 하도록 solution 함수를 완성해주세요.
>
> ### **[제한사항]**
>
> - 징검다리를 건너야 하는 니니즈 친구들의 수는 무제한 이라고 간주합니다.
> - stones 배열의 크기는 1 이상 200,000 이하입니다.
> - stones 배열 각 원소들의 값은 1 이상 200,000,000 이하인 자연수입니다.
> - k는 1 이상 stones의 길이 이하인 자연수입니다.

## 풀이 1 (실패)

처음에는 무한 반복문을 통해 더 이상 최대 k개를 건너뛸 수 없을 때까지 한 명씩 징검다리를 건너도록 코드를 짰다.

결론적으로 효율성 테스트에서 시간 초과가 났는데, 생각해보면 시간 복잡도가 $O(n^2)$이 나오므로 그닥 효율적인 코드라고 할수는 없다.

```cpp
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> stones, int k) {
    int answer = 0;

    while(1) {
        int currentPosition = -1;
        while(1) {
            bool isFinished = false;

            for(int i = 1; i <= k; i++) {
                int nextPosition = currentPosition + i;

                // 다 건넌 경우
                if(nextPosition >= stones.size()) {
                    answer++;
                    isFinished = true;
                    break;
                }

                // nextPosition을 밟을 수 있는 경우
                if(stones[nextPosition] > 0) {
                    stones[nextPosition]--;
                    currentPosition = nextPosition;
                    break;
                }

                // 건널 수 없는 경우
                if(i == k) {
                    return answer;
                }
            }

            if(isFinished) {
                break;
            }
        }
    }

    return answer;
}
```

## 풀이 2 (실패)

다음으로는 DP를 통해 풀이를 시도했으나 역시 효율성 테스트에서 시간 초과가 났다. 1번 풀이보다는 상황에 따라 효율적이나 k가 stones 배열의 크기에 가까울 시, 시간 복잡도가 $O(n^2)$로 역시 효율적이지 못한 코드가 된다.

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<int> stones, int k) {
    int answer = 0;

    vector<int> DP(stones.size());

    // i번 디딤돌까지 최대 몇명이 건널 수 있는지

    for(int i = 0; i < stones.size(); i++) {
        int maxValue;
        if(i - k < 0) {
            maxValue = stones[i];
        } else {
            maxValue = 0;
        }

        for(int j = i - 1; j >= 0 && j >= i - k; j--) {
            maxValue = max(maxValue, DP[j]);
        }

        DP[i] = min(maxValue, stones[i]);
    }

    for(int i = stones.size() - 1; i >= 0 && i >= stones.size() - k; i--) {
        answer = max(answer, DP[i]);
    }

    return answer;
}
```

## 풀이 3 (성공)

알고보니 이진 탐색으로 풀 수 있던 문제였다. k가 최대 200,000이므로 $log_2{200000}$는 대략 17~18 사이의 값이 나오므로 left 값과 right 값을 각각 0과 stones.size() - 1로 설정해 이진 탐색을 수행하면 효율성 테스트를 통과할 수 있게 된다.

min 값과 max 값을 더하고 2로 나눠 mid 값을 구한 뒤, mid 값을 건너는 인원으로 설정해 모든 디딤돌의 값에서 뺀다.

디딤돌의 값이 0보다 적을 시, 건너 뛰어야 한다는 소리가 되는데 만약 연속으로 건너 뛰어야 하는 디딤돌의 수가 k개보다 많을 시, answer 값이 현재 mid 값보다 낮다는 의미가 되므로 right 값을 mid-1로 설정한다.

반면 디딤돌의 수가 k개보다 적거나 같을 시, answer를 현재 answer 값과 현재 mid 값 중 최댓값으로 바꾸고 left를 mid + 1로 수정해 계속 탐색을 진행한다.

이를 left가 right 값보다 커질 때까지 수행하면 된다.

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<int> stones, int k) {
    int answer = 0;
    int left = 0, right = 200000000;

    // n^2보다 적어야 함
    // 이진 탐색 (nlogn)
    while(left <= right) {
        int mid = (left + right) / 2;
        int skipCount = 0;
        bool isPossible = true;

        vector<int> tmp(stones);

        for(int i = 0; i < tmp.size(); i++) {
            tmp[i] -= mid;

            if(tmp[i] < 0) {
                skipCount++;
            } else {
                skipCount = 0;
            }

            if(skipCount >= k) {
                isPossible = false;
                break;
            }
        }

        if(isPossible) {
            answer = max(answer, mid);
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    return answer;
}
```
