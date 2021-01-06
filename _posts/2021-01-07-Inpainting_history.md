---
title: "인페인팅 기법들이 발전해온 과정"
categories:
    - JARA
# layout: default
---


인페인팅 기법들이 발전해온 과정

> Barnes가 patch-match 알고리즘 제안

복원할 영역 주변에서 가장 적합한 패치를 찾아서 영역을 채운다.

*C. Barnes, et.al. "Patchmatch: A randomized correspondence algorithm forstructural image editing" ACM Transactions on Graphics, 28 (2009)*

> Wilczkowiak가 patch-match 알고리즘을 발전시킴

복원에 사용할 가장 적합한 패치를 찾을 영역을 찾는다.

*M. Wilczkowiak, et.al. "Hole filling through photomonatage" BMVC (2005)*

Barnes와 Wilczkowiak의 patch-match 알고리즘은 고차원 semantics에 대한 이해가 부족하고 지역적으로 유일한 패턴을 재구성하는데 어려움이 있다.

---

> 딥러닝 기반 인페인팅 기법

영상의 semantic 정보를 얻기 위하여 데이터 분포를 학습하여 그럴듯한 인페인팅 결과를 얻는다. 문맥 정보를 제대로 이용하지 못해서 때때로 인페인팅된 영역에 노이즈 패턴이 나타난다. 

*S. Iizuka, et.al. "Globally and locally consistent image completion" ACM Transactions on graphics, 44(4) (2014)*

---

> 픽셀들 사이의 관계에 초점을 맞춘 접근 기법

1. Spatial attention을 이용한 기법

    마스크 주변 픽셀 정보들을 이용해 인페인팅을 수행한다. 사각형 모양의 마스크 처리에만 집중한 연구이고 픽셀 간의 연결성이 떨어지고 semantic 균열이 발생하는 단점이 있다.

    *Y. Song, et.al. "Contextual-based image inpainting: Infer, match, and translate" ECCV (2018)*

2. 원본 영상의 valid pixel에 기반하여 missing pixel을 생성하는 기법

    다양한 마스크에 대한 인페인팅을 수행할 수 있다. Semantic 균열이 발생하고 테두리를 구성하는 픽셀 불균형이 발생하는 문제가 있다. 

    *G. Liu, et.al. "Image inpainting for irregular holes using partial convolutions" ECCV (2018)*

전범위적인 Semantic 연결성과 생성된 픽셀 간의 연결성을 고려하지 않고 지엽적으로 픽셀 간 연결성만을 고려했기 때문에 위와 같은 문제가 발생한다.

---

> 그림 복원 기법에서 착안한 인페인팅 기법

두 단계를 통한 인페인팅을 통해 전범위적인 구조의 연결성과 지엽적인 픽셀 간의 연결성을 확보

1. Conception (구상)

    전범위적인 구조 파악

2. Painting (색칠)

    선을 새로 그리거나 색칠

> CSA (Coherent Semantic Attention) layer 제안

물체에 가려져서(occluded) 모르는 영역의 모르는 feature 패치를 알고있는 영역의 가장 유사한 feature 패치로 초기화한다. 초기화된 feature 패치는 인접한 패치들과 공간적 연결성을 고려하여 최적화한다. 최적화는 여러 번의 반복을 통해 수행된다. 전범위적인 구조의 연결성은 첫 번째 단계에서 확보된다. 지엽적인 feature 일관성은 두 번째(최적화 단계)에서 확보된다.

*Hongyu Liu, et.al. "Coherent Semantic Attention for Image Inpainting" arXiv: 1905.12384 (2019)*