---
title: "비전 트랜스포머(Vision Transformer)"
categories:
    - Deep Learning
# layout: default
---
![Vision Transformer](https://github.com/developerTae/developerTae.github.io/assets/46318721/a3e41b8b-9c0d-4b22-9443-c538614098d9)

컴퓨터 비전에서 NLP 분야의 트랜스포머를 적용한 네트워크입니다. CNN만 사용하는 네트워크보다 더 우수한 성능을 보입니다. ViT는 논문에서 제시한 대로 NLP 분야의 트랜스포머 인코더 부분을 그대로 사용합니다. CNN을 사용하지 않고도 Vision 태스크에서 좋은 성능을 낼 수 있습니다. 그러나 충분한 데이터셋으로 사전 학습하고 타겟 도메인에 fine-tuning해야 성능을 제대로 발휘합니다.

논문 저자들은 이미지 패치를 만들어 1D 임베딩을 수행합니다. 예를 들어 [300, 300, 3] 크기의 이미지를 [100, 100, 3] 크기의 9개 패치로 만듭니다.

각 이미지 패치를 1차원으로 만듭니다(linear projection). 하나의 패치가 [100, 100, 3]이었다면 각 픽셀을 일렬로 이어 붙여서 [1, 100x100x3] 차원의 1차원으로 만듭니다.

BERT의 CLS token과 유사하게, 임베딩된 패치의 맨 앞에 하나의 학습 가능한 class token을 추가했습니다. 이 class token은 트랜스포머의 여러 인코더층을 거쳐 최종 출력으로 나왔을 때, 이미지에 대한 1차원 representation 벡터의 역할을 합니다.

1차원으로 만든 벡터에 같은 차원의 position embedding을 더해줍니다. Position embedding은 NLP의 트랜스포머와 유사하게 임베딩에 순서 정보를 부여하기 위한 것입니다.

이미지 패치와 position embedding을 사용하여 셀프 어텐션을 수행하기 위해, 임베딩 1개당 1개씩의 쿼리(query), 키(key), 밸류(value)를 구합니다. 예를 들어 num_heads를 12로 설정하면 각 임베딩마다 [1, 100x100x3/12] 차원의 쿼리, 키, 밸류를 갖게 됩니다. 이렇게 구한 쿼리, 키, 밸류를 사용하여 어텐션 값을 구하고 이들을 concatenate하여 멀티 헤드 어텐션을 만듭니다.

---

참고문헌

[1] [한땀한땀 딥러닝 컴퓨터 비전 백과사전](https://wikidocs.net/137253){: target="_blank"}