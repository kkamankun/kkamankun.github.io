---
title: "MaxViT: Multi-Axis Vision Transformer (ECCV 2022)"
categories:
    - Paper Review
# layout: default
use_math: true
---
본 논문에서 제안하는 'Multi-axis self attention(Max-SA)'은 새로운 종류의 트랜스포머 모듈입니다. 이 모듈은 blocked local과 dilated global 어텐션으로 구성됩니다. 모듈 내에서 지역적 및 전역적 공간 상호작용을 수행할 수 있습니다.

본 논문에서는 이 모듈의 효과와 범용성을 입증하기 위해 'Multi-axis Vision Transformer(MaxViT)'라는 vision backbone을 제안합니다. MaxViT는 계층적으로 Max-SA와 Convolution으로 구성된 블록을 반복해서 쌓아 구성되었습니다. MaxViT는 다양한 태스크에서 최고 수준의 결과를 달성하는 등, 넓은 범위의 태스크에서 탁월한 성능을 보여줍니다.

MaxViT은 MBConv, 블록 및 그리드 어텐션 레이어를 통합하는 새로운 유형의 블록으로 구성됩니다. 아래 그림은 MaxViT의 구조를 나타냅니다. 정규화 및 활성화 레이어는 생략하여 구조를 단순화했습니다.

![MaxViT](https://github.com/developerTae/developerTae.github.io/assets/46318721/4b2d18fb-1164-4710-98b0-cb6127c8e6e0)

Max-SA 모듈은 완전한 밀집 어텐션 메커니즘을 윈도우 어텐션과 그리드 어텐션으로 분해합니다. 이 방법을 통해 vanilla 어텐션의 제곱 복잡도를 선형 복잡도로 줄이면서도 non-locality를 손실 없이 유지할 수 있습니다.

아래 그림은 윈도우/그리드 크기가 4x4인 Max-SA 모듈의 예시를 보여줍니다. 블록 어텐션 모듈은 윈도우 내에서 셀프 어텐션을 수행하며, 그리드 어텐션 모듈은 균일 그리드에 있는 전체 2D 공간에 오버레이된 픽셀 전체에 대해 전역적으로 어텐션을 수행합니다. 두 모듈 모두 고정된 어텐션 footage를 사용하므로 입력 크기에 대해 선형적인 복잡성을 가지며, 같은 색상은 셀프 어텐션 연산에 의해 공간적으로 혼합됩니다.

![Max-SA](https://github.com/developerTae/developerTae.github.io/assets/46318721/1e93ff62-ada2-4224-b526-8e9ea1467002)

셀프 어텐션은 로컬 합성곱과 달리 전역 상호작용이 가능한 장점이 있습니다. 그러나 어텐션 연산자가 제곱 복잡도를 요구하기 때문에 전체 공간에 대해 직접 어텐션을 적용하는 것은 계산상 불가능합니다. 이 문제를 해결하기 위해, 본 논문은 공간 축을 분해하여 로컬과 전역 두 가지 형태로 전체 크기의 어텐션을 분해합니다. 예를 들어, 입력 feature map인  $X\in \mathbb{R}^{H\times W\times C}$이 있다면, $P \times P$ 크기의 겹치지 않는 윈도우로 분할하여 $\left ( \frac{H}{P} \times \frac{W}{P}, P\times P, C \right )$ 모양의 텐서로 피쳐를 블록화합니다. 이러한 블록 어텐션은 local interactions에 사용됩니다.

또한, 이 논문은 sparse global attention을 간단하면서도 효과적으로 얻기 위해 그리드 어텐션을 제안합니다. Feature map을 고정된 윈도우 크기로 나누는 것 대신에, $G \times G$ 그리드를 사용하여, 텐서를 $\left ( G\times G, \frac{H}{G}\times \frac{W}{G}, C\right )$ shape으로 분해합니다. 이를 통해, $\frac{H}{G}\times \frac{W}{G}$ 크기의 그리드를 얻을 수 있습니다. 분해된 그리드 축 $G \times G$에 셀프 어텐션을 적용하는 것은 토큰의 확장된 전역 공간 혼합을 의미합니다. 이는 마스킹, 패딩 또는 순환 이동이 필요하지 않으므로 글로벌 상호 작용 능력을 갖추고 있으며, 구현에 더 친화적입니다.

이 논문에서는 블록 내에서 로컬 및 전역 상호작용을 얻기 위해 두 종류의 어텐션을 순차적으로 수행하며, MBConv 블록에는 어텐션과 함께 스퀴즈-앤-익스사이트(SE) 모듈이 추가됩니다. 이는 네트워크의 학습 능력 뿐만 아니라 일반화 성능을 높이는 데 도움이 됩니다. Max-SA는 지역 상호작용을 위한 블록 어텐션과 전역적인 혼합을 위한 그리드 어텐션, 두 가지 다양한 목적으로 함께 또는 분리하여 사용할 수 있습니다.

본 논문에서는 Max-ViT를 다양한 비전 태스크에서 평가했습니다. ImageNet classification, image object detection, instance segmentation, image aesthetics/quality assessment, unconditional image generation 등이 포함됩니다. Table 10에 따르면, 순차적으로 수행하는 방식은 매우 적은 매개 변수와 계산으로 병렬 방식보다 뛰어난 성능을 보여줍니다. 이는 병렬 디자인이 서로간의 상호작용이 적은 보충적인 신호를 배우는 반면, 순차적 스택은 지역적 및 전역적 계층 간 보다 강력한 퓨전을 학습할 수 있기 때문입니다.

![Table 10](https://github.com/developerTae/developerTae.github.io/assets/46318721/f6bc4a24-7ed8-468e-acd2-31b2bc2474b1)

보다 구체적인 정보를 위해서는 논문 링크를 참고해주세요. 다음은 논문 링크입니다.

[arXiv](https://arxiv.org/abs/2204.01697){: target="_blank"}