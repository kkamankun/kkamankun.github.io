---
title: "[머신러닝] 1. Entire Machine Learning Flow"
categories:
    - Computer Science
# layout: default
---
> 본 포스팅은 광운대학교 정용진 교수님의 '머신러닝'을 수강하며 정리한 노트입니다.

End to end 머신러닝에 대해 이해해보자. End to end란 처음부터 끝까지라는 의미로 데이터(입력)에서 목표한 결과(출력)를 사람이 개입 없이 얻는다는 뜻이다.

**문제 정의 → 데이터 수집 → 데이터 분석 → 모델 학습 → 모델 테스트 → 배포** 의 단계로 전체 과정이 구성된다. 데이터 분석 단계에 대해 다루겠다. 데이터 분석 단계에서 수행하는 작업들은 다음과 같다.

1. analysis, x와 y의 correlation, chiz test, graph로 그려보기
2. missing value가 있는 항목 처리
    1. 해당 샘플 제거하기
    2. 해당 feature 제거하기
    3. 다른 값으로 대체하기
3. categorical encoding (관측 결과가 몇 개의 범주 또는 항목의 형태로 나타나는 자료)
    1. interger encoding (범주형 자료를 수치형 자료로 표현)
    2. ordinal encoding (순위를 고려)
    3. label encoding
    4. one-hot encoding
4. 데이터의 분포를 적용하여 scaling (normalize)

모델 학습과 모델 테스트에 대해 다루겠다. 학습 데이터, 테스트 데이터, 실생활 데이터의 분포를 표준 정규 분포로 scaling할 때는 학습 데이터들의 평균과 표준편차를 사용해야 한다. 위 내용을 실습 코드로 확인해본다. 다음은 실습 코드를 이해하는데 필요한 개념들이다.

- lab34a_scikit_learn_object.ipynb

_mean_and_std() 함수: 데이터 X에 대한 평균과 표준 편차 반환

my_StandardScaler() 클래스의 transfrom() 메소드: 데이터 x를 표준 정규 분포로 변환

모델 생성  → 데이터 분석 → 데이터 변환 → 예측 순서로 진행

- Handson_02_end_to_end_machine_learning_project_rev1.ipynb

pandas.read_csv("housing.csv"): data