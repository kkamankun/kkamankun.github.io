---
title: "[유형별 기출문제] 이진 탐색"
categories:
    - Python
# layout: default
---
> 본 포스팅은 나동빈 저자의 '이것이 취업을 위한 코딩테스트'를 공부하며 정리한 노트입니다.

Q27. 정렬된 배열에서 특정 수의 개수 구하기
---

```python
from bisect import bisect_left, bisect_right
import sys

# 값이 [left_value, right_value]인 데이터를 반환하는 함수
def count_by_range(a, left_value, right_value):
    right_index = bisect_right(a, right_value)
    left_index = bisect_left(a, left_value)
    return right_index - left_index

n, x = map(int, sys.stdin.readline().rstrip().split())
array = list(map(int, sys.stdin.readline().rstrip().split()))

# 이진 탐색 수행 결과 출력
result = count_by_range(array, x, x)
if result == 0:
    print(-1)
else:
    print(result)
```

오름차순으로 정렬된 수열이 주어진다. 수열의 원소 중에서 값이 x인 원소의 개수를 출력하라. 단, 시간 복잡도는 O(logN)으로 알고리즘을 설계해야 한다.

문제 해결 방법: 원소들이 정렬되어 있기 때문에, 수열 내에 x가 존재한다면 연속적으로 나열되어 있을 것이다. 따라서 x가 처음 등장하는 인덱스와 x가 마지막으로 등장하는 인덱스를 각각 계산한 뒤에, 그 인덱스 차이를 계산하여 문제를 해결한다. 파이썬의 이진 탐색 라이브러리인 bisect를 활용하여 해결했다.

Q28. 고정점 찾기
---

```python
# 이진 탐색 소스코드 구현(재귀 함수)
def binary_search(array, start, end):
    if start > end:
        return None
    mid = (start + end) // 2
    # 찾은 경우 중간점 인덱스 반환
    if array[mid] == mid:
        return mid
    # 찾고자 하는 값이 중간점보다 작은 경우 왼쪽 확인
    elif array[mid] > mid:
        return binary_search(array, start, mid - 1)
    # 찾고자 하는 값이 중간점보다 큰 경우 오른쪽 확인
    else:
        return binary_search(array, mid + 1, end)

# n(원소의 개수)와 전체 원소 입력받기
n = int(input())
array = list(map(int, input().split()))

# 이진 탐색 수행 결과 출력
result = binary_search(array, 0, n - 1)
if result is None:
    print(-1)
else:
    print(result)
```

모든 원소가 오름차순 정렬된 수열이 주어진다. 수열의 원소 중에서 그 값이 인덱스와 동일한 원소(고정점)가 있다면, 고정점을 출력하라.  단, 시간 복잡도는 O(logN)으로 알고리즘을 설계해야 한다.

문제 해결 방법: '찾고자 하는 값'이 '중간점'과 동일하다고 가정하고, 이진 탐색을 수행한다.

Q29. 공유기 설치
---

```python
import sys

# 집의 개수(N)와 공유기의 개수(C)를 입력받기
n, c = map(int, sys.stdin.readline().rstrip().split())
# 각 집의 좌표 정보를 입력받기
array = []  # 집의 좌표의 값은 최대 10억까지의 정수
for _ in range(n):
    array.append(int(sys.stdin.readline()))
array.sort()

# 이진 탐색을 위한 시작점과 끝점 설정
end = array[-1] - array[0]  # 인접한 두 집 사의의 최대 거리
start = 1  # 인접한 두 집 사의의 최소 거리

# 이진 탐색 수행(반복적)
result = 0
while start <= end:
    value = array[0]
    count = 1
    mid = (start + end) // 2  # gap을 조정하며 공유기를 설치해보기
    for i in range(1, n):
        if array[i] >= value + mid:
            value = array[i]
            count += 1
    if count >= c:  # 공유기를 c개보다 더 설치할 수 있으면 gap을 늘려보기
        result = mid
        start = mid + 1
    else:  # 공유기를 c개보다 덜 설치할 수 있으면 gap을 줄여보기
        end = mid - 1
print(result)
```

C개(최대 20만)의 공유기를 N개(최대 20만)의 집에 적당히 설치해서, 가장 인접한 두 공유기 사이의 거리를 최대로 하는 프로그램을 작성하라.

문제 유형: 파라메트릭 서치

문제 해결 방법: 각 집의 좌표가 최대 10억(탐색 범위가 10억)이므로, 이진 탐색을 이용하여 문제를 해결한다. '가장 인접한 두 공유기 사이의 최대 거리'를 조절해가며,  매 순간 공유기를 설치하여 C보다 많은 개수로 공유기를 설치할 수 있는지 체크한다.

Q30. 가사 검색
---

```python
from bisect import bisect_left, bisect_right

def count_by_range(a, left_value, right_value):
    right_index = bisect_right(a, right_value)
    left_index = bisect_left(a, left_value)
    return right_index - left_index

def solution(words, queries):
    word_len = dict()
    for w in words:
        try:
            word_len[len(w)].append(w)
        except KeyError:
            word_len[len(w)] = [w]
    for k in word_len.keys():
        word_len[k].sort()
    _word_len = dict()
    for k in word_len.keys():
        for x in word_len[k]:
            try:
                _word_len[-k].append(x[::-1])
            except KeyError:
                _word_len[-k] = [x[::-1]]
    for k in _word_len.keys():
        _word_len[k].sort()
    answer = []
    for q in queries:
        n = len(q)
        left_value = q.replace('?', 'a')
        right_value = q.replace('?', 'z')
        if q[0] == '?':  # prefix
            try:
                answer.append(count_by_range(_word_len[-n], left_value[::-1], right_value[::-1]))
            except KeyError:
                answer.append(0)
        else:  # suffix
            try:
                answer.append(count_by_range(word_len[n], left_value, right_value))
            except KeyError:
                answer.append(0)

    return answer
```

입력: 단어들이 담긴 배열 words, 키워드가 담긴 배열 queries

출력: 각 키워드별로 매치된 단어가 몇 개인지 순서대로 담은 배열

키워드 매치 조건: 키워드에 포함된 와일드카드 문자인 '?'는 글자 하나를 의미하며, 어떤 문자에도 매치됨

문제 해결 방법:  두 배열 words와 queries를 각각 순차 탐색으로 비교하면 시간 제한에 맞게 문제를 해결할 수 없다. 따라서 모든 원소를 정렬시킨 뒤에 이진 탐색을 이용하여 O(logN)으로 값을 찾아낸다.

해결 순서:

1. 각 단어를 길이에 따라 나누기(사전 자료형 - 키: 단어의 길이, 값: 키값에 해당하는 길이를 갖는 단어 리스트)
2. 모든 단어 리스트 정렬
3. 각 쿼리에 대해서 이진 탐색 수행