---
title: "[프로그래머스] 2020 카카오 인턴십 키패드 누르기"
categories:
    - Programmers
# layout: default
---
문제
---

입력으로 순서대로 누를 번호가 담긴 배열 numbers와 왼손잡이인지 오른손잡이인 지를 나타내는 문자열 ("lelf" 또는 "right") hand가 주어진다. 문제에서 주어진 규칙에 맞게 순서대로 번호를 누른다. 각 번호를 누른 손가락이 왼손이면 L, 오른손이면 R을 순서대로 이어붙여 연속된 문자열 형태로 반환하라.

어떻게 풀 것인가?
---

- 사전 자료형을 사용

키: 눌러야 할 숫자
값: 좌표(거리 계산할 때 쓰려고)

- 두 키패드의 위치가 주어질 때, 맨해튼 거리를 계산하여 반환하는 distance 함수를 정의

맨해튼 거리는 도시의 골목길을 걸을 때의 모습과 유사하기 때문에 붙은 거라고 한다.

```python
def distance(location1, location2):
    return abs(location2[0] - location1[0]) + abs(location2[1] - location1[1])

def solution(numbers, hand):
    data = dict()
    data['1'] = [0, 0]
    data['2'] = [0, 1]
    data['3'] = [0, 2]
    data['4'] = [1, 0]
    data['5'] = [1, 1]
    data['6'] = [1, 2]
    data['7'] = [2, 0]
    data['8'] = [2, 1]
    data['9'] = [2, 2]
    data['*'] = [3, 0]
    data['0'] = [3, 1]
    data['#'] = [3, 2]
    
    curL = data['*']
    curR = data['#']
    
    answer = ''
    for i in range(len(numbers)):
        if numbers[i] == 1 or numbers[i] == 4 or numbers[i] == 7:
            curL = data[str(numbers[i])]
            answer += 'L'
        elif numbers[i] == 3 or numbers[i] == 6 or numbers[i] == 9:
            curR = data[str(numbers[i])]
            answer += 'R'
        else:  # 두 엄지손가락의 현재 키패드의 위치에서 더 가까운 엄지손가락을 사용한다.
            if distance(curL, data[str(numbers[i])]) < distance(curR, data[str(numbers[i])]):
                curL = data[str(numbers[i])]
                answer += 'L'
            elif distance(curL, data[str(numbers[i])]) > distance(curR, data[str(numbers[i])]):
                curR = data[str(numbers[i])]
                answer += 'R'
            else:  # 두 엄지손가락의 거리가 같다면,
                if hand == "left":
                    curL = data[str(numbers[i])]
                    answer += 'L'
                else:  # hand == "right"
                    curR = data[str(numbers[i])]
                    answer += 'R'
    return answer
```