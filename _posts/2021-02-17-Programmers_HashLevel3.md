---
title: "[프로그래머스] 해시 Level3 베스트앨범"
categories:
    - Programmers
# layout: default
---
문제
---

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 구현하라.

어떻게 풀 것인가?
---

사전 자료형을 사용한다.

```python
def solution(genres, plays):
    answer = []
    genres_set = set(genres)
    data = dict()  # 키: 노래의 장르, 값: 속한 노래의 재생 횟수
    for s in genres_set:
        tmp = []
        for i in range(len(genres)):    
            if s == genres[i]:
                tmp.append(plays[i])
        data[s] = sum(tmp)
    
    # 키를 기준으로 내림차순 정렬
    data_val_reverse = sorted(data.items(), reverse=True, key=lambda item: item[1])

    for key, value in data_val_reverse:
        data2 = dict()   # 키: 고유 번호, 값: 노래가 재생된 횟수
        for i in range(len(genres)):
            if key == genres[i]:
                data2[i] = plays[i]
        
        # 키를 기준으로 내림차순 정렬
        data_val_reverse2 = sorted(data2.items(), reverse=True, key=lambda item: item[1])        
        
        cnt = 0
        for key2, value2 in data_val_reverse2:
            answer.append(key2)
            cnt += 1
            if cnt == 2:
                break
        
    return answer
```