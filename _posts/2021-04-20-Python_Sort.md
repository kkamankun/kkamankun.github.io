---
title: "[유형별 기출문제] 정렬"
categories:
    - Python
# layout: default
---
> 본 포스팅은 나동빈 저자의 '이것이 취업을 위한 코딩테스트'를 공부하며 정리한 노트입니다.

Q23. 국영수
---

```python
# 나의 풀이
n = int(input())
array = []
for _ in range(n):
    temp = list(input().split())
    dic = dict()
    dic['이름'] = temp[0]
    dic['국어'] = int(temp[1])
    dic['영어'] = int(temp[2])
    dic['수학'] = int(temp[3])
    array.append(dic)
array.sort(key=lambda x: (-x['국어'], x['영어'], -x['수학'], x['이름']))
for data in array:
    print(data['이름'])

    
# 책의 풀이
n = int(input())
students = []  # 학생 정보를 담을 리스트

# 모든 학생 정보를 입력받기
for _ in range(n):
    students.append(input().split())

'''
[정렬 기준]
1) 두 번째 원소를 기준으로 내림차순 정렬
2) 두 번째 원소가 같은 경우, 세 번째 원소를 기준으로 오름차순 정렬
3) 세 번째 원소가 같은 경우, 네 번째 원소를 기준으로 내림차순 정렬
3) 네 번째 원소가 같은 경우, 첫 번째 원소를 기준으로 오름차순 정렬
'''
students.sort(key=lambda x: (-int(x[1]), int(x[2]), -int(x[3]), x[0]))

# 정렬된 학생 정보에서 이름만 출력
for student in students:
    print(student[0])
```

입력: N명 학생의 이름, 국어, 영어, 수학 점수 (N은 최대 100,000)

정렬 조건:

1. 국어 점수가 감소하는 순서로
2. 영어 점수가 증가하는 순서로
3. 수학 점수가 감소하는 순서로
4. 이름이 사전 순으로 증가하는 순서로

문제 해결 방법: sort() 함수의 key 속성에 값을 대입하여 내가 원하는 '조건'에 맞게 원소들을 정렬시킨다.

Q24. 안테나
---

```python
# 나의 풀이
import sys

n = int(sys.stdin.readline().rstrip())
input_data = list(map(int, sys.stdin.readline().rstrip().split()))
input_data.sort()
cnt = 0
for i in input_data:
    cnt += 1
    if cnt >= n // 2:
        break
print(i)

# 책의 풀이
n = int(input())
data = list(map(int, input().split()))
data.sort()

# 중간값(median)을 출력
print(data[(n - 1) // 2])
```

입력: 집의 수 N, N 채 집의 위치 (N은 최대 이십만)

안테나로부터 모든 집까지의 거리의 총합이 최소가 되도록 안테나를 설치

출력: 안테나를 설치할 위치의 값

문제 유형: 수학

문제 해결 방법: 정확히 중간값에 해당하는 위치의 집에 안테나를 설치했을 때, 안테나로부터 모든 집까지의 거리의 총합이 최소가 된다.

Q25. 실패율
---

```python
# 나의 풀이
def solution(N, stages):
    answer = []
    t = len(stages)
    for i in range(1, N + 1):
        cnt = stages.count(i)

        if t == 0:
            fail = 0
        else:
            fail = cnt / t  # 실패율 계산
        answer.append((i, fail))
        t -= cnt

    answer.sort(key=lambda x: x[1], reverse=True)
    return [i[0] for i in answer]

# 책의 풀이
def solution(N, stages):
    answer = []
    length = len(stages)

    # 스테이지 번호를 1부터 N까지 증가시키며
    for i in range(1, N + 1):
        # 해당 스테이지에 머물러 있는 사람의 수 계산
        count = stages.count(i)

        # 실패율 계산
        if length == 0:
            fail = 0
        else:
            fail = count / length

        # 리스트에 (스테이지 번호, 실패율) 원소 삽입
        answer.append((i, fail))
        length -= count

    # 실패율을 기준으로 각 스테이지를 내림차순 정렬
    answer = sorted(answer, key=lambda t: t[1], reverse=True)

    # 정렬된 스테이지 번호 출력
    answer = [i[0] for i in answer]
    return answer
```

입력: 전체 스테이지의 개수 N, 게임을 이용하는 사용자가 현재 멈춰있는 스테이지의 번호가 담긴 배열 stages (N은 최대 20만)

출력: 실패율이 높은 스테이지부터 내림차순으로 스테이지 번호 출력

문제 유형: 구현, 정렬

문제 해결 방법: 스테이지 번호(i)를 1부터 N까지 증가시키며, 해당 단계에 머물러 있는 플레이어들의 수(count)를 계산한다. 그러한 플레이어들의 수(count) 정보를 이용하여 모든 스테이지에 따른 실패율을 계산한 뒤에 저장한다.

Q26. 카드 정렬하기
---

```python
# 나의 풀이
from queue import PriorityQueue
que = PriorityQueue()
n = int(input())
for i in range(n):
    que.put(int(input()))

cnt = []
for i in range(n-1):
    s = que.get() + que.get()
    cnt.append(s)
    que.put(s)

print(sum(cnt))

# 책의 풀이
import heapq

n = int(input())

# 힙(Heap)에 초기 카드 묶음을 모두 삽입
heap = []
for i in range(n):
    data = int(input())
    heapq.heappush(heap, data)

result = 0

# 힙(Heap)에 원소가 1개 남을 때까지
while len(heap) != 1:
    # 가장 작은 2개의 카드 묶음 꺼내기
    one = heapq.heappop(heap)
    two = heapq.heappop(heap)
    # 카드 묶음을 합쳐서 다시 삽입
    sum_value = one + two
    result += sum_value
    heapq.heappush(heap, sum_value)

print(result)
```

입력: 숫자 카드 묶음의 개수 N, 숫자 카드 묶음의 각각의 크기 (N은 최대 10만)

주어진 숫자 카드 묶음을 합치려면 비교가 필요

출력: 주어진 카드 묶음을 모두 합치기 위한 최소 비교 횟수

문제 유형: 그리디, 정렬

문제 해결 방법: 매 상황에서 가장 작은 크기의 두 카드 묶음을 꺼내서 이를 합친 뒤에 다시 리스트에 삽입한다. 이러한 과정을 수행하기 위해여 우선순위 큐를 사용한다.