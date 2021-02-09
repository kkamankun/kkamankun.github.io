---
title: "[프로그래머스] 2020 카카오 인턴십 보석 쇼핑"
categories:
    - Programmers
# layout: default
---
문제
---

이름이 다른 여러 종류의 번호 순으로 진열대에 보관되어 있다. 진열된 모든 종류의 보석을 적어도 1개 이상 포함하는 가장 짧은 구간을 찾아서 반환하라.

어떻게 풀 것인가?
---

1. 이중 반복문으로 발생 가능한 모든 조합을 구한다.

```python
def solution(gems):
    s1 = set(gems)
    n = len(s1)
    
    temp = []
    for i in range(len(gems)):
        for j in range(i+1, len(gems)+1):
            s2 = set(gems[i:j])
            if len(s2) == n:
                temp.append([i+1, j])       
    
    min = temp[0][1] - temp[0][0]
    for e in temp:
        if min > e[1] - e[0]:
            min = e[1] - e[0]
    
    answer = []
    for e in temp:
        if e[1] - e[0] == min:
            answer.append(e)

    return answer[0]
```

2. 중복을 허용하지 않는 집합 자료형의 특징을 활용한다.

```python
def solution(gems):
    start = 0
    end = len(set(gems))
    temp = []
    
    while start < end <= len(gems):
        bag = set(gems[start:end])
        
        if len(bag) < len(set(gems)):
            end += 1
        else:
            start += 1
            temp.append([start, end])
        
    answer = [0, 0]
    min = float('inf')
    for s, e in temp:
        if min > e - s:
            min = e - s
            answer[0] = s
            answer[1] = e
    return answer
```

3. 중복을 허용하지 않는 사전 자료형의 특징을 활용한다.

```python
def solution(gems):
    n = len(set(gems))  # 보석의 종류
    start = 0  # 윈도우의 시작점
    end = 0  # 윈도우의 끝점

    bag = dict()  # 보석을 담을 가방
    for i in gems[start:end]:
        if i in bag:
            bag[i] += 1
        else:
            bag[i] = 1

    candidate = []
    while start <= end <= len(gems):
        if len(bag) < n:
            if end == len(gems):
                break
            if gems[end] in bag:  # 새로 잡은 보석이 가방에 있다면,
                bag[gems[end]] += 1  # 개수를 추가
            else:  # 새로 잡은 보석이 가방에 없다면,
                bag[gems[end]] = 1  # 새로운 키-값 초기화
            end += 1
        else:  # 모든 종류의 보석을 담았다면
            candidate.append([start, end])  # 시작인덱스와 끝인덱스 저장
            bag[gems[start]] -= 1
            if bag[gems[start]] == 0:
                bag.pop(gems[start])
            start += 1

    answer = [0, 0]
    d = float('inf')
    for s, e in candidate:
        if d > e - s:
            d = e - s
            answer[0] = s + 1
            answer[1] = e
    return answer
```