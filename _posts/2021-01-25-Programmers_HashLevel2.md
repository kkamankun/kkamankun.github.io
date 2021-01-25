---
title: "[프로그래머스] 해시 Level2 전화번호 목록"
categories:
    - Programmers
# layout: default
---
문제
---

전화번호부에 적힌 전화번호를 담은 배열이 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있어면 false를 그렇지 않으면 true를 return 하도록 함수를 작성하라.

어떻게 접근할 것인가?
---

이 문제의 풀이는 대략 세 가지 방법이 있다.

1. 리스트 정렬
2. 해시
3. 트라이

해시를 활용해서 문제를 풀고 싶었는데 해시를 사용할 생각이 떠오르지 않아 **리스트 정렬**로 풀었다. 주어진 리스트를 오름차순 정렬한다. startswith() 함수를 사용하여 접두어인 경우를 판별한다.

```python
def solution(phone_book):
    n = len(phone_book)
    phone_book.sort()
    for i in range(n-1):
        if phone_book[i+1].startswith(phone_book[i]) == True:
            return False
    answer = True
    return answer
```