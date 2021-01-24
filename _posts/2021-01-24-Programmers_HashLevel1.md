---
title: "[프로그래머스] 해시 Level1 완주하지 못한 선수"
categories:
    - Programmers
# layout: default
---
문제
---

마라톤에 참가한 선수들의 이름을 알고 있다. 이후 완주한 선수들의 이름이 주어졌을 때, 완주하지 못한 선수의 이름을 어떻게 빠르게 알 수 있을까?

어떻게 접근할 것인가?
---

완주한 선수들의 이름만 사전 자료구조에 담는다. 사전 자료형과 관련된 함수를 이용하여 해결한다. 사전 자료형에 특정한 원소가 있는지 검사할 때는 '원소 in 사전'의 형태를 사용한다. 이는 리스트나 튜플에 대해서도 사용할 수 있다.

사전 자료형에서는 키(Key)는 고유한 값이므로 중복되는 키 값을 설정해 놓으면 하나를 제외한 나머지 것들이 무시된다는 점을 주의해야 한다.

```python
def solution(participant, completion):
    n = len(participant)
    data = dict()
    for i in range(n - 1):
        data[completion[i]] = 0

    for i in range(n):
        if participant[i] in data:
            data[participant[i]] += 1

    for i in range(n):
        if participant[i] in data:
            continue
        else:
            answer = participant[i]
        return answer

    for i in range(n - 1):
        if data.get(completion[i]) != completion.count(completion[i]):
            answer = completion[i]
            return answer
```