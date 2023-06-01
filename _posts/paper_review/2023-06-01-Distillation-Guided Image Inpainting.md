---
title: "Distillation-Guided Image Inpainting (ICCV 2021)"
categories:
    - Paper Review
# layout: default
use_math: true
---
최근 딥러닝을 이용한 이미지 복원 기술에서 image inpainting 태스크의 성능이 크게 향상되었습니다. 하지만 이러한 기술은 hole 영역에 왜곡된 구조나 균일하지 않은 텍스처를 생성하는 문제가 있습니다. 이는 encoder 레이어의 missing region에 대한 embedding 성능과 관련된 문제입니다. 이전의 coarse-to-fine, progressive refinement 및 structural guidance 등의 기술은 다중 생성 네트워크, feature의 제한된 성능, GT 정보의 최적 활용에 따른 연산량 오버헤드 문제가 있습니다.

본 논문에서는 feature level distillation을 활용한 새로운 학습 방법을 제안합니다. Cross와 self-distillation 기법, 그리고 hole에 대한 보다 정확한 encoding이 가능한 completion-block을 도입하였습니다. 또한 attention transfer와 pixel-adaptive global-local feature fusion을 결합한 attention module을 활용하여 일관성을 향상시키는 방법을 제안합니다. 실험 결과, 제안하는 기법은 SOTA를 크게 뛰어넘는 성능을 보였으며, 기존의 다른 inpainting 방법들에도 성능 향상을 가져올 수 있음을 입증하였습니다.

Distillation은 이미지 분류, 이미지 분할, 이미지 복원, 캡션 생성 등에서 주로 사용됩니다. 이를 위해, 보다 유익한 정보를 제공하는 teacher를 활용하여 student model의 성능을 향상시키는 다양한 방법들이 제안되고 있습니다. 이 중에서도, 중간 representation을 추가적인 guidance로 활용하는 방법과 teacher들의 ensemble 및 계단식 distillation 등이 제안되었습니다. 최근 몇 연구에서는, 동일한 task를 수행하는 teacher network 내에서 distilling하는 것이 student의 성능을 더욱 높일 수 있다는 것이 입증되었습니다. 본 연구는 세 가지 종류의 distillation을 네트워크의 서로 다른 부분에 적용하여, 성능을 향상시키는 방법을 제안합니다.

- (a) 두 네트워크의 encoder-feature들 사이의 distillation
- (b) 동일한 네트워크 내에서 encoder-feature들 사이의 distillation
- (c) Decoder-feature 대신 어텐션 모듈의 affinity-finding behavior를 distillation하는 방법

![network](https://github.com/developerTae/developerTae.github.io/assets/46318721/2e009e85-5755-4181-9efc-665fe8029ee9)

두 네트워크의 encoder-feature들 사이의 distillation loss는 다음과 같이 구성됩니다. 

![Encoder-Feature Distillation Loss](https://github.com/developerTae/developerTae.github.io/assets/46318721/77d30bd9-9e3e-4e36-bc30-c2d42d474434){: width="40%"",height="40%""}

입력을 $x$로, 복원 네트워크의 파라미터를 $\theta$로 나타내며, inpainting-encoder의 출력을 $x_l$로, auxiliary-encoder의 출력을 $x_{l}^{\ast}$로 표기합니다. 여기서 $l$은 레이어를 의미합니다. 마스크를 뜻하는 $M$은 1은 hole을, 0은 그 외의 지역을 나타냅니다. $\gamma$는 메타 네트워크를 의미합니다. 메타 네트워크는 컨볼루션 연산으로 구성되며 $x_{l}^{\ast}$을 복원 태스크에 더 도움이 되도록 변형합니다.

두 네트워크의 encoder-feature들 사이의 distillation은 전체 영역에 대한 더 완전한 목표를 제공하지만, 보다 얕은 레이어들은 이를 처음에는 달성하기 어려울 수 있습니다. 따라서 동일한 네트워크 내에서의 distillation은 이러한 레이어들을 위해 더 유용하고 달성 가능한 목표를 제공합니다. 동일한 네트워크 내에서 encoder-feature들 사이의 distillation은 다음과 같이 구성됩니다.

![Self-Distillation Loss](https://github.com/developerTae/developerTae.github.io/assets/46318721/8b1ed2ac-33b7-4851-b1e7-7ec37f74c52d){: width="55%"",height="55%""}

$x_l$은 $l^{th}$ 레벨의 encoder-feature를 의미하며, $f_l$은 $x_l$을 $x_{l+1}$과 같은 차원으로 만들어주는 컨볼루션 레이어를 뜻합니다. 또한, $\phi^l_c$는 음수가 아닌 채널 가중치를 의미합니다. $l$은 $1$과 $L-1$ 사이의 숫자입니다.

Auxiliary network(AN)에서 어텐션 모듈의 유사성 검색 동작을 inpainting 모델의 어텐션 모듈이 모방하도록 하기 위해, decoder-feature 대신 AN과 inpainting network(IN)에서 어텐션 모듈을 배치합니다. AN의 feature는 손상되지 않았기 때문에, 서로 다른 패치/픽셀 간의 측정된 pairwise 유사성은 hole 영역에 대해서도 정확합니다. 어텐션 모듈은 컨볼루션 기반 어텐션 모듈 대신 행렬 곱셈 기반 연산을 사용하여 동등한 성능을 발휘하면서 훨씬 효율적으로 픽셀 유사도를 계산합니다. Decoder-feature $d_l$이 레벨 $l$에서 주어졌을 때, 쌍으로 된 가중치 (pairwise weightage)는 다음과 같이 계산됩니다.

![Pairwise-Weightage](https://github.com/developerTae/developerTae.github.io/assets/46318721/61086a6f-fd2c-4832-aa1a-df275950c562){: width="30%"",height="30%""}

여기서 $p_{i,j}=f_Q(d^i)^Tf_K(d^j)$, $f_Q$와 $f_K$는 1x1 컨볼루션 레이어이며, $N$은 픽셀의 총 개수를 나타냅니다. 또한 $d$는 입력 feature입니다. 이 어텐션 레이어의 출력은 다음과 같습니다.

![Attention Layer](https://github.com/developerTae/developerTae.github.io/assets/46318721/356380a3-58f3-4d43-9242-97b74463a353){: width="18%"",height="18%""}

AN 분기에서 학습한 관계 $r_{i,j}^{AN}$가 있다면, IN의 유사도 행렬$(r_{i,j})$과 AN의 유사도 행렬$(r_{i,j}^{AN})$ 간의 거리를 최소화합니다. 또한, 각 픽셀은 메타 네트워크 $t$를 사용하여 AN 네트워크에서 학습된 유사도의 상대적 중요도를 적응적으로 선택할 수 있습니다.

이러한 지식을 이용하여 inpainting 모델을 유사하게 동작하도록 만들어 attention transfer loss를 계산합니다.

![Attention Transfer Loss](https://github.com/developerTae/developerTae.github.io/assets/46318721/c2703d0d-6f66-48bc-8ca6-9bd231b77535){: width="40%"",height="40%""}

$H$는 구멍(Hole) 영역을 뜻하며, $t_i$는 메타 트워크로 0과 1 사이의 값으로, 시그모이드 연산이 이어지는 하나의 컨볼루션 레이어를 사용하여 생성됩니다.

보다 구체적인 정보를 위해서는 논문 링크를 참고해주세요. 다음은 논문 링크입니다.

[IEEE Xplore](https://ieeexplore.ieee.org/document/9710869){: target="_blank"}