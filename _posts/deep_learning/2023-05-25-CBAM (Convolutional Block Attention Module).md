---
title: "CBAM (Convolutional Block Attention Module)"
categories:
    - Deep Learning
# layout: default
use_math: true
---
CBAM은 attention module을 사용하여 모델의 성능을 향상시킵니다. Channel attention module과 spatial attention module로 구성되어 있으며, 각각의 attention module은 채널과 공간에 대한 attention map을 생성합니다. 생성한 attention map을 input feature map에 곱하여 필요없는 정보를 억제하고 중요한 정보를 강조합니다. 정리하자면, CBAM은 input feature map에서 채널과 공간 정보에 대한 attention map을 생성하여 모델이 어디에 집중해야 하는지에 대한 정보를 제공합니다. SENet에서는 채널별 가중치를 계산하고, residual attention network에서는 공간 픽셀별 가중치를 계산합니다. 이러한 개념을 통합한 것이 CBAM입니다.

Channel attention module은 입력 feature에서 1D 채널 어텐션 맵을 생성합니다. 입력 feature F의 내부 채널 관계를 활용하여 채널 어텐션 맵을 생성합니다. 이를 위해 입력 feature map의 공간 차원을 1x1로 압축하여 효과적으로 계산합니다. 즉, Cx1x1이 됩니다. 또한 공간 정보를 통합하기 위해 평균 풀링과 최대 풀링을 적용합니다. 두 pooling 연산을 함께 사용하면 성능이 향상됩니다.

$M_c(F)=\sigma(MLP(AvgPool(F))+MLP(MaxPool(F)))$

![Channel Attention Module](https://github.com/developerTae/developerTae.github.io/assets/46318721/2827c67d-850d-4d47-9815-36946286a12f)

Spatial attention module은 2D 공간 어텐션 맵을 생성합니다. 이 모듈은 어디에 중요한 정보가 있는지 집중하도록 합니다. 채널 어텐션 맵과 입력 feature map을 곱하여 생성한 F'에서 채널을 축으로 Maxpool과 Avgpool을 적용하여 생성한 1xHxW의 F_avg와 F_max의 두 값을 concatenate합니다. 이 결과에 7x7 conv 연산을 적용하여 공간 어텐션 맵을 생성합니다.

$M_s(F)=\sigma(f^{7*7}([AvgPool(F);MaxPool(F)]))$

![Spatial Attention Module](https://github.com/developerTae/developerTae.github.io/assets/46318721/f8a50cfe-19dc-4b17-b386-78d988ea8fc1)

---

참고문헌

[1] https://arxiv.org/abs/1807.06521

[2] https://deep-learning-study.tistory.com/666