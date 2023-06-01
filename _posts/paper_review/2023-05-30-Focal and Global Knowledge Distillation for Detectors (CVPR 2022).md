---
title: "Focal and Global Knowledge Distillation for Detectors (CVPR 2022)"
categories:
    - Paper Review
# layout: default
use_math: true
---
본 논문에서는 아래와 같은 새로운 내용들을 제안합니다.

- Focal and Global Distillation(FGD)를 통해 영상을 foreground와 background로 구분하여 각각 distillation을 수행합니다.
- Teacher network와 student network의 spatial과 channel attention을 구하여 어떤 픽셀과 채널에 집중해야 하는지 distillation을 수행합니다.
- Foreground와 background 불균형 문제로 인해 knowledge distillation을 적용하는데 어려움이 있던 object detection 연구에 성공적으로 적용되었습니다.

논문에 대한 간략한 소개입니다. Object detection 태스크에서 teacher와 student의 feature가 영역 별로 다르기 때문에 전체 영상에 대해 distillation을 수행하는 것은 적절하지 않습니다. 따라서 본 논문에서는 foreground와 background를 구분하여 distillation을 수행합니다. 이를 위해 focal과 global distillation을 제안합니다. Focal distillation은 GT box를 사용해 영상의 foreground와 background를 구분하고 attention mask를 사용하여 student가 teacher가 집중하고 있는 중요한 픽셀과 채널들에 집중하도록 합니다. Global distillation은 서로 다른 픽셀들 사이의 관계를 추출하고 이를 teacher로부터 student로 전달합니다. 이를 통해 focal distillation에서 고려하지 않은 global information을 보완합니다.

논문의 개요에 대한 설명입니다. Knowledge distillation은 큰 teacher network로부터 compact한 student network로 정보를 전달하는 기법입니다. 이를 통해 네트워크 구조 변경을 하지 않고도 더 우수한 성능을 얻을 수 있습니다.

본 논문에서는 Teacher와 student의 feature의 차이를 시각화하여 spatial과 channel attention map으로 보여주었습니다. Fig. 1을 보면, foreground에서 student와 teacher의 attention에 큰 차이가 있는 것을 알 수 있습니다. 반면, background에서의 attention 차이는 상대적으로 작습니다. 이러한 상황은 foreground와 background를 학습하는데 어려움을 초래합니다.

![Spatial and Channel Attention Map](https://github.com/developerTae/developerTae.github.io/assets/46318721/b0757e15-2bda-4e6e-af53-6a8c8a2b1cc2){: width="60%"",height="60%""}

추가적으로, foreground와 background가 knowledge distillation에 미치는 영향을 파악하기 위해 두 영역을 구분하여 실험을 진행하였습니다. Table.1을 보면, 전체 영상에 대해 distillation한 경우보다는 foreground와 background를 구분하여 distillation한 경우가 성능이 더 우수하였음을 알 수 있습니다.

![Comparisions of Different Distillation Areas](https://github.com/developerTae/developerTae.github.io/assets/46318721/e985b305-9b51-4366-a673-58d59bd79f70){: width="60%"",height="60%""}

따라서, 이 논문에서는 focal distillation을 제안합니다. 이를 위해 foreground와 background를 분리하고, teacher의 channel과 spatial attention을 추출하여 student에 전달합니다.

Object detection 태스크에서는 global context가 중요합니다. 그러나, focal distillation에서는 foreground와 background가 분리되어 있기 때문에 global information이 충분히 반영되지 않을 수 있습니다. 이를 극복하기 위해 global distillation을 제안합니다. 이를 위해 GcBlock을 사용하여 픽셀들 사이의 관계를 출력하고, 이를 teacher에서 student로 전달합니다. 본 논문에서는 feature들로만 손실 함수를 구성하였습니다.

네트워크 구조는 focal distillation과 global distillation으로 구성됩니다.

![An Illustration of FGD](https://github.com/developerTae/developerTae.github.io/assets/46318721/7ca2430f-3b74-4db7-9c0b-22f543bfe99a)

Focal Distillation은 영상의 local 정보를 전달하는 과정으로, 3개의 mask를 생성하는 것이 핵심입니다. Binary Mask는 foreground와 background를 분리하여 GT box에는 1을, 그 외에는 0을 설정합니다. ($r$은 GT box, $i$와 $j$는 feature map의 좌표)

![Binary Mask](https://github.com/developerTae/developerTae.github.io/assets/46318721/103dc82a-053b-4547-9f9f-93ceacec610f){: width="30%"",height="30%""}

Scale Mask는 영상 내 target의 크기가 다양한 경우에 대처하기 위한 방법입니다. 크기가 큰 물체는 더 많은 픽셀을 차지하고, 손실 함수에서도 더 높은 가중치를 가지기 때문에, 크기가 작은 물체에 대한 학습에 영향을 미칠 수 있습니다. 이를 방지하기 위해, Foreground에 대응하는 손실 값을 계산할 때, GT box의 면적으로 나누어 계산합니다. 만약 여러 물체가 동일한 픽셀을 공유한다면, 가장 작은 box의 높이와 너비를 사용합니다. ($H_r$과 $W_r$은 GT box의 높이와 너비)

![Scale Mask](https://github.com/developerTae/developerTae.github.io/assets/46318721/cd850056-c8c3-4a76-8da3-a3f3ef5abe56){: width="30%"",height="30%""}

Attention Mask는 teacher의 feature map에서 추출한 spatial과 channel attention 값을 가중치로 사용하여, student가 knowledge distillation을 수행하도록 유도합니다. 

![Attention Map](https://github.com/developerTae/developerTae.github.io/assets/46318721/f9f85723-5263-4ea3-91e9-1c189dbb241a){: width="30%"",height="30%""}

attention map ($S$는 spatial attention, $C$는 channel attention)

![Attention Mask](https://github.com/developerTae/developerTae.github.io/assets/46318721/a98337dd-b2e4-4168-967c-197ec4fea45e){: width="40%"",height="40%""}

attention mask ($T$는 temperature 상수)

Feature loss는 다음과 같이 정리할 수 있습니다. ($\alpha$와 $\beta$는 가중치)

![Feature Loss](https://github.com/developerTae/developerTae.github.io/assets/46318721/7608c774-425c-412c-92b4-565014c6ee5f){: width="60%"",height="60%""}

또한, student에서 생성된 attention map과 teacher의 attention map을 최대한 유사하게 만들기 위해 attention loss를 사용합니다. ($\gamma$는 가중치, $l$은 L1 loss, $t$는 teacher, $s$는 student)

![Attention Loss](https://github.com/developerTae/developerTae.github.io/assets/46318721/cfe4627e-2d8b-48ba-ae43-3c6e3e6930e1){: width="35%"",height="35%""}

위 둘을 더한 것이 최종적인 focal loss입니다.

Global Distillation은 focal distillation에서 고려하지 못한 서로 다른 픽셀 간의 관계 정보를 보강하기 위해 사용되며, GcBlock을 사용하여 영상의 관계 정보를 추출합니다.

![Global Distillation](https://github.com/developerTae/developerTae.github.io/assets/46318721/49be3f6a-0d33-42a6-b419-d12e12c55821){: width="60%"",height="60%""}

Global loss는 다음과 같습니다. ($F^T$와 $F^S$의 관계 사이의 평균 제곱 오차,  $\lambda$는 가중치)

![Global Loss](https://github.com/developerTae/developerTae.github.io/assets/46318721/27ccbeb8-eaa9-442d-91d4-b3307206a4b8){: width="40%"",height="40%""}

총 손실 함수는 아래와 같습니다. ($L_{original}$은 student의 예측 결과와 GT 사이의 loss)

![Total Loss](https://github.com/developerTae/developerTae.github.io/assets/46318721/4ce6756e-c0e6-42f6-8c1d-a4b37dc6833f){: width="35%"",height="35%""}

Distillation loss는 detector의 neck 부분(예: Feature Pyramid Network)에서만 계산되므로, 다양한 detector에 쉽게 적용할 수 있습니다.

주요 실험 결과는 FGD를 통해 distillation을 수행한 후에, student의 spatial과 channel attention이 teacher와 매우 유사하게 변화하였음을 확인할 수 있습니다. 이를 통해, student가 teacher의 knowledge를 학습하여 더 좋은 feature를 생성하고 성능을 향상시키는 데 도움이 되는 것으로 나타났습니다.

![Experiments](https://github.com/developerTae/developerTae.github.io/assets/46318721/4b6db649-0bf9-4bf2-9f47-791aa3796604)

보다 구체적인 정보를 위해서는 논문 링크를 참고해주세요. 다음은 논문 링크입니다.

[CVPR 2022 open access](https://openaccess.thecvf.com/content/CVPR2022/html/Yang_Focal_and_Global_Knowledge_Distillation_for_Detectors_CVPR_2022_paper.html){: target="_blank"}
