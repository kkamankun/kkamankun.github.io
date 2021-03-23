---
title: "[유형별 기출문제] 그리디 문제"
categories:
    - Python
# layout: default
---
> 본 포스팅은 나동빈 저자의 '이것이 취업을 위한 코딩테스트'를 공부하며 정리한 노트입니다.

그리디: 현재 상황에서 가장 좋아 보이는 것만을 선택하는 알고리즘

Q1. 모험가 길드

---

1. 각 모험가의 공포도의 값을 오름차순 정렬한다.
2. 공포도가 낮은 모험가부터 그룹에 포함한다.
3. 만약, 현재 그룹에 포함된 모험가의 수가 현재 확인하고 있는 공포도보다 크거나 같다면, 그룹을 결성한다.

정렬을 통해 그룹에 포함된 모험가의 수를 항상 최소가 되도록 그룹을 결성할 수 있다.

```python
n = int(input())
input_data = list(map(int, input().split()))
input_data.sort()
cnt = 0
result = 0
for i in input_data:
  cnt += 1
  if cnt >= i:
    result += 1
    cnt = 0
print(result)
```

Q2. 곱하기 혹은 더하기

---

확인하는 숫자가 0 또는 1인 경우를 제외하고는 곱셈을 수행하는 것이 수를 크게 만들 수 있다. 확인하고 숫자가 0 또는 1인 경우에는 더하기를 수행한다.

```python
# 나의 풀이
s = input()
result = int(s[0])
for i in range(1, len(s)):
  if result == 0 or result == 1:
    result += int(s[i])
  else:
    result *= int(s[i])
print(result)

# 답지 풀이
s = input()
result = int(s[0])
for i in range(1, len(s)):
  num = int(s[i])
  if result <= 1 or num <= 1:
    result += num
  else:
    result *= num
print(result)
```

Q3. 문자열 뒤집기

---

문자열의 숫자를 모두 0으로 바꾸기 위해 숫자를 뒤집는 횟수와 모두 1로 바꾸기 위해 뒤집는 횟수를 비교하여 더 작은 값을 출력한다.

```python
# 답지 풀이
s = input()
cnt0, cnt1 = 0, 0

if s[0] == '0':
  cnt1 += 1
else:
  cnt0 += 1

for i in range(1, len(s)):
  if s[i-1] != s[i]:
    if s[i] == '1':
      cnt0 += 1
    else:
      cnt1 += 1
      
print(min(cnt0, cnt1))
```

Q4. 만들 수 없는 금액

---

현재 확인하는 동전을 이용해 target 금액을 만들 수 있는지 확인한다. 동전을 화폐 단위 기준으로 정렬한 뒤에, 화폐 단위가 작은 동전들부터 하나씩 확인하면서 target 변수를 업데이트한다.

1. input_data.sort()
2. target = 1
3. for i in input_data:
4.   if target ≥ i:
5.     target += i

```python
# 나의 코드
n = int(input())
input_data = list(map(int, input().split()))
input_data.sort()
cnt_list = [0] * 100
for i in range(0, n - 1):
  for j in range(i + 1, n):
    cnt_list[input_data[i] + input_data[j]] += 1
for i in range(0, n):
  if cnt_list[i] == 0:
    print(i + 1)
    break

# 책 코드
'''
현재 상태를 '1부터 target-1까지의 모든 금액을 만들 수 있는 상태'라고 보자.
이때 매번 target인 금액도 만들 수 있는지 체크하는 것이다.
만약 해당 금액을 만들 수 있다면, target의 값을 업데이트하면 된다.
'''
n = int(input())
input_data = list(map(int, input().split()))
input_data.sort()
target = 1
for i in input_data:
  if target >= i:
    target += i
print(target)
```

Q5. 볼링공 고르기

---

A가 고른 볼링공의 무게와 다른 무게의 볼링공을 B가 고를 수 있는 조합의 수를 구한다. 이를 구현하기 위해 각 무게별로 볼링공이 몇 개 존재하는지 저장하는 리스트를 선언한다.

```python
# 나의 코드
import itertools

n, m = map(int, input().split())
input_data = list(map(int, input().split()))
temp = []
for i in input_data:
  if i <= m:
    temp.append(i)
result = list(itertools.combinations(temp, 2))
for x in result:
  if x[0] == x[1]:
    result.remove(x)
print(len(result))

# 책 풀이
n, m = map(int, input().split())
input_data = list(map(int, input().split()))
# 볼링공의 무게별 개수를 저장할 배열 선언
arr = [0] * 11
for i in input_data:
    arr[i] += 1
result = 0
# 첫 번째 사람이 볼링공을 고르면, 두 번째 사람이 다른 무게의 볼링공을 고르는 조합의 수를 계산
for i in range(1, m+1):  # 공의 무게가 1부터 m까지
    n -= arr[i]
    result += arr[i] * n
print(result)
```

Q6. 무지의 먹방 라이브

---

음식을 먹는 데 걸리는 시간을 기준을 정렬한 뒤 시간이 적게 걸리는 음식부터 제거한다. 남아있는 음식들 중에 다음으로 섭취해야 할 가장 가까운 번호의 음식을 찾는다.

```python
# 나의 코드
def solution(food_times, k):
    cur_food = 0
    for i in range(k):
        food_times[cur_food] -= 1
        cur_food += 1
        if cur_food == len(food_times):
            cur_food = 0
        else:
            while food_times[cur_food] == 0:
                cur_food += 1
                if cur_food == len(food_times):
                    cur_food = 0
                    break
    answer = cur_food
    return answer + 1

# 책의 풀이
import heapq  # 우선순위 큐(Min 힙)

def solution(food_times, k):
    if sum(food_times) <= k:
        return -1
    q = []
    # 모든 음식을 시간을 기준을 정렬
    for i in range(len(food_times)):
        heapq.heappush(q, (food_times[i], i+1))  # (각 음식을 먹는 데 필요한 시간, 해당 음식의 번호)
    sum_value = 0  # 먹기 위해 사용한 시간
    previous = 0  # 이전 음식을 먹는 데 필요했던 시간
    length = len(food_times)  # 남아 있는 음식의 개수
    while sum_value + (q[0][0] - previous) * length <= k:
        now = heapq.heappop(q)[0]  # 현재 음식을 먹는 데 필요한 시간
        sum_value += (now - previous) * length
        length -= 1
        previous = now
    # 음식의 번호 순서로 남아있는 음식들을 정렬
    result = sorted(q, key=lambda x: x[1])
    return result[(k - sum_value) % length][1]

print(solution([3, 1, 2], 5))
```