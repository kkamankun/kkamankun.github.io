---
title: "[유형별 기출문제] 구현"
categories:
    - Python
# layout: default
---
> 본 포스팅은 나동빈 저자의 '이것이 취업을 위한 코딩테스트'를 공부하며 정리한 노트입니다.

Q7. 럭키 스트레이트

---

```python
s = input()
n = len(s)
t1, t2 = 0, 0
for i in range(n//2):
  t1 += int(s[i])
for j in range(n//2, n):
  t2 += int(s[j])
if t1 == t2:
  print('LUCKY')
else:
  print('READY')
```

Q8. 문자열 재정렬

---

```python
from collections import deque

s = list(input())
s.sort()
q = deque(s)
t = 0
while 48 <= ord(q[0]) <= 57:
  t += int(q[0])
  q.popleft()
answer = ''
for x in list(q):
  answer += x
print(answer + str(t))
```

Q9. 문자열 압축

---

```python
def solution(s):
  cnt = 1
  min = len(s)
  for unit in range(1, len(s)):  # 한 단계씩 증가시키며 확인
    result = ''
    for i in range(0, len(s), unit):
      if s[i:i+unit] != s[i+unit:i+(2*unit)]:
        if cnt > 1:
          result += str(cnt)
        result += s[i:i+unit]
        cnt = 1
      else:
        cnt += 1
    if len(result) < min:
      min = len(result)
  return min
```

Q10. 자물쇠와 열쇠

---

```python
def rotate_a_matrix_by_90_degree(a):
  n = len(a)
  m = len(a)
  result = [[0] * n for _ in range(m)]
  for i in range(n):
    for j in range(m):
      result[j][n - i - 1] = a[i][j]
  return result

def check(new_lock):
  lock_length = len(new_lock) // 3
  for i in range(lock_length, lock_length * 2):
    for j in range(lock_length, lock_length * 2):
      if new_lock[i][j] != 1:
        return False
  return True

def solution(key, lock):
  n = len(lock)
  m = len(key)
  new_lock = [[0] * (n * 3) for _ in range(n * 3)]
  for i in range(n):
    for j in range(n):
      new_lock[i + n][j + n] = lock[i][j]

  for rotation in range(4):
    key = rotate_a_matrix_by_90_degree(key)
    for x in range(n * 2):
      for y in range(n * 2):
        for i in range(m):
          for j in range(m):
            new_lock[x + i][y + j] += key[i][j]
        if check(new_lock) == True:
          return True
        for i in range(m):
          for j in range(m):
            new_lock[x + i][y + j] -= key[i][j]
  return False

print(solution([[0, 0, 0], [1, 0, 0], [0, 1, 1]], [[1, 1, 1], [1, 1, 0], [1, 0, 1]]))
```

Q11. 뱀

---

```python
n = int(input())  # 보드의 크기
k = int(input())  # 사과의 개수
data = [[0] * (n + 1) for _ in range(n + 1)]  # 맵 정보
info = []  # 방향 회전 정보

# 맵 정보(사과 있는 곳은 1로 표시)
for _ in range(k):
  a, b = map(int, input().split())
  data[a][b] = 1

# 방향 회전 정보 입력
l = int(input())
for _ in range(l):
  x, c = input().split()
  info.append((int(x), c))

# 처음에는 오른쪽을 보고 있으므로(동, 남, 서, 북)
dx = [0, 1, 0, -1]
dy = [1, 0, -1, 0]

def turn(direction, c):
  if c == "L":
    direction = (direction - 1) % 4
  else:
    direction = (direction + 1) % 4
  return direction

def simulate():
  x, y = 1, 1  # 뱀의 머리 위치
  data[x][y] = 2  # 뱀이 존재하는 위치는 2로 표시
  direction = 0  # 처음에는 동쪽을 보고 있음
  time = 0  # 시작한 뒤에 지난 '초' 시간
  index = 0  # 다음에 회전할 정보
  q = [(x, y)]  # 뱀이 차지하고 있는 위치 정보(꼬리가 앞쪽)
  while True:
    nx = x + dx[direction]
    ny = y + dy[direction]
    # 맵 범위 안에 있고, 뱀의 몸통이 없는 위치라면
    if 1 <= nx and nx <= n and 1 <= ny and ny <= n and data[nx][ny] != 2:
      # 사과가 없다면 이동 후에 꼬리 제거
      if data[nx][ny] == 0:
        data[nx][ny] = 2
        q.append((nx, ny))
        px, py = q.pop(0)
        data[px][py] = 0
      # 사과가 있다면 이동 후에 꼬리 그대로 두기
      if data[nx][ny] == 1:
        data[nx][ny] = 2
        q.append((nx, ny))
    # 벽이나 뱀의 몸통과 부딪혔다면
    else:
      time += 1
      break
    x, y = nx, ny  # 다음 위치로 머리를 이동
    time += 1
    if index < l and time == info[index][0]:  # 회전할 시간인 경우 회전
      direction = turn(direction, info[index][1])
      index += 1
  return time

print(simulate())
```

Q12. 기둥과 보 설치

---

```python
def possible(answer):
    for x, y, stuff in answer:
        if stuff == 0:  # 기둥
            if y == 0 or [x - 1, y, 1] in answer or [x, y, 1] in answer or [x, y - 1, 0] in answer:
                continue
            return False
        elif stuff == 1:  # 보
            if [x, y - 1, 0] in answer or [x + 1, y - 1, 0] in answer or ([x - 1, y, 1] in answer and [x + 1, y, 1] in answer):
                continue
            return False
    return True

def solution(n, build_frame):
    answer = []
    for frame in build_frame:
        x, y, stuff, operate = frame
        if operate == 0:
            answer.remove([x, y, stuff])
            if not possible(answer):
                answer.append([x, y, stuff])
        if operate == 1:
            answer.append([x, y, stuff])
            if not possible(answer):
                answer.remove([x, y, stuff])
    return sorted(answer)
```

Q13. 치킨 배달

---

```python
# 나의 풀이
from itertools import combinations

def manhattan_distance(pt1, pt2):  # 점1, 점2
    result = 0
    for i in range(len(pt1)):
        result += abs(pt2[i] - pt1[i])
    return result

n, m = map(int, input().split())  # 마을의 크기: n, 최대 치킨집의 개수: m
town = []
for _ in range(n):
    town.append(list(map(int, input().split())))
home_coordinate, chicken_coordinate = [], []  # 인덱스 1부터 시작하게 만들기 위함
for i in range(n):
    for j in range(n):
        if town[i][j] == 1:  # 가정집 좌표 저장
            home_coordinate.append([i, j])
        elif town[i][j] == 2:  # 초기 치킨집의 좌표 저장
            chicken_coordinate.append([i, j])
        else:
            pass
home_cnt = len(home_coordinate)
chicken_cnt = len(chicken_coordinate)
combinations_result = list(combinations(chicken_coordinate, m))

# M개의 치킨집을 뽑을 수 있는 모든 경우의 수를 따져보면서
min_chickenDistance = float("inf")
for i in combinations_result:
    chickenDistance = 0
    for j in range(home_cnt):  # 가정집을 하나씩 기준으로
        min_homeToChicken = float("inf")
        for k in range(m):  # 가장 가까운 곳에 위치한 치킨집으로의 치킨거리의 맨하탄 거리를 구하여
            dist = manhattan_distance(home_coordinate[j], i[k])
            min_homeToChicken = min(min_homeToChicken, dist)
        chickenDistance += min_homeToChicken  # 치킨거리의 합을 구하기
    min_chickenDistance = min(min_chickenDistance, chickenDistance)
    
# 결과 출력
answer = min_chickenDistance
print(answer)
© 2021 GitHub, Inc.
```

Q14. 외벽 점검

---

```python
# 나의 
def solution(n, weak, dist):
    # 점검표 초기화
    checkList = [1] * n  # 1: 점검 완료, 0: 점검 필요
    for x in weak:
        if x == n:
            checkList[0] = 0
        else:
            checkList[x] = 0

    dist.sort(reverse=True)  # 점검 능력이 뛰어난 점검자부터 점검 내보내기
    checkInfo = dict()

    # 점검자 순차적으로 내보내기
    cnt = 0
    for x in dist:  # 점검 능력이 뛰어난 점검자부터 점검 내보내기
        cnt += 1
        max_checkCnt = 0
        for y in weak:  # 최적의 점검을 할 수 있도록 모든 점검 시작 지점 따져보기
            # 왼쪽으로 점검할 때 점검 예정인 지점 개수
            print(y-x, y+1)
            if y - x < 0:
                checkL = checkList[0:y+1].count(0)
            else:
                checkL = checkList[y-x:y+1].count(0)
            # 오른쪽으로 점검할 때 점검 예정인 지점 개수
            if y + x >= n:
                checkR = checkList[y:n].count(0)
                checkR += checkList[0:y+x-n+1].count(0)
            else:
                checkR = checkList[y:y+x+1].count(0)
            # 두 경우 중에 더 많이 점검할 수 있는 경우로 점검 예정
            if checkR > checkL:
                direction = 'R'
                checkCnt = checkR
            else:
                direction = 'L'
                checkCnt = checkL
            print('시작지점, 왼쪽, 오른쪽: ', y, checkL, checkR)
            if max_checkCnt < checkCnt:  # 가장 많이 점검할 수 있는 시작점과 방향 저장
                max_checkCnt = checkCnt
                checkInfo[x] = (y, direction)
        print('checkList: ', checkList)

        # 얻은 출발점과 점검 방향 정보를 바탕으로 점검 시작
        start, checkDir = checkInfo[x][0], checkInfo[x][1]
        print('start, dir: ', start, checkDir)
        if checkDir == 'R':
            if start + x < n:
                for i in range(start, start + x + 1):
                    checkList[i] = 1
            else:
                for i in range(start, n):
                    checkList[i] = 1
                for i in range(0, start + x - n + 1):
                    checkList[i] = 1
        else:  # checkDir == 'L'
            for i in range(start - x, start + 1):
                checkList[i] = 1

        # 점검표 점검
        if checkList.count(1) == n:  # 외벽 점검을 모두 완료했다면,
            answer = cnt  # 점검자 수를 반환
            return answer
    # 모든 점검자로 점검이 되지 않는 경우
    return -1

print(solution(12, [1, 5, 6, 10], [1, 2, 3, 4]))
# print(solution(12, [1, 3, 4, 9, 10], [3, 5, 7]))
```