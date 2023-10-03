---
title: "Uncertainty-Aware Unsupervised Image Deblurring with Deep Residual Prior (CVPR 2023)"
categories:
    - Paper Review
# layout: default
use_math: true
---
이미지 디블러는 다음과 같은 컨볼루션 연산으로 표현할 수 있습니다:

\[
y=k\otimes x+n
\]

위 수식에서 $y$와 $x$는 각각 블러 이미지와 선명한 이미지를 뜻하고, $k$는 블러 커널, $n$은 가우시안 노이즈를 뜻합니다. 커널의 이용 가능성에 따라 디블러 방법은 커널을 모른다고 가정하는 blind 디블러와 커널이 알려졌다고 가정하는 non-blind 디블러 방법으로 구분됩니다. 전형적인 디블러 방법들은 2단계로 진행됩니다: 1) 블러 이미지로부터 블러 커널을 추정하기, 2) 추정된 블러 커널을 통해 선명한 이미지를 복구하기. Non-blind 디블러 방법은 커널에 오류가 없는 경우에만 잘 동작하는 한계가 있습니다.

따라서, 커널 오류 개선을 위해 사전 정보(prior)를 제공하는 semi-blind 방법이 연구되고 있습니다. 이때 사용되는 사전 정보는 hand-crafted와 data-driven 방식으로 구분됩니다. 그러나 각 방식 모두 한계점이 존재합니다.

본 논문에서는 deep residual prior(DRP)라는 dataset-free deep prior를 제안합니다. 이 방법은 학습되지 않은 딥러닝 네트워크를 활용하여 실제 환경에서 발생하는 복잡한 residual을 포착합니다. 

먼저, semi-blind 디블러 문제를 수식으로 정리했습니다. 커널 오류와 발생 가능한 아티팩트를 고려했습니다:

\[
y=\left ( \hat{k} + \Delta k\right )\otimes x+h+n= \hat{k}\otimes x+r+h+n
\]

위 수식에서 $y$와 $x$는 각각 블러 이미지와 선명한 이미지를 뜻하고,  $\hat{k}$와 $\Delta k$는 각각 블러 커널과 커널 오류, $r=\Delta k\otimes x$는 커널 오류로부터 유도된 residual, $h$는 아티팩트를 뜻합니다. 블러 이미지로부터 $x, h, r$을 추정하는 것은 ill-posed problem이므로 선택지를 좁히기 위해서 사전 정보가 필요합니다.  

이를 위해 DRP와 sparse prior를 통합하여 $r$, deep image prior(DIP)와 total variation을 통합하여 $x$, sparse prior와 discrete cosine transform(DCT)을 통합하여 $h$의 사전 정보를 얻습니다.

Residual $r$의 복잡한 구조를 유연하고 견고하게 모델링하기 위해 학습되지 않은 딥러닝 네트워크를 활용합니다. 또한, 공간 도메인에서 residual은 sparse 하기 때문에, sparse prior를 DRP의 guide로 활용합니다. 

선명한 이미지 $x$의 모델링을 위해 DIP를 활용합니다. DIP는 선명한 이미지의 통계를 이용하기 때문에, zero-shot 시나리오에서 여러 블러들과 이미지를 처리하는데 적합합니다. 또한, local smoothness를 위해 total variation을 DIP의 guide로 활용합니다.

아티팩트 $h$의 모델링을 위해 DCT 도메인에서 sparse prior를 활용합니다. 커널 오류 $\Delta k$ 때문에 발생하는 아티팩트는 edge 주변에서 주기성을 나타내고, 이는 DCT 계수가 sparse 하다는 것을 의미하기 때문입니다. 

![Deep_Residual_Prior](https://github.com/kkamankun/kkamankun.github.io/assets/46318721/19b4fdb2-dcd2-4d0d-90a3-398ae412cb39)

DIP 모듈은 랜덤 노이즈 입력 $z_{x}$을 입력받아 이미지를, DRP 모듈은 $z_{r}$를 입력받아 residual을 추정합니다. 

정리하면, 본 논문에서는 학습되지 않은 딥러닝 네트워크로 residual을 예측하는 dataset free DRP를 제안했으며, 이를 통해 실제 시나리오에서 여러 블러들과 이미지에 대한 일반화를 가능하게 했습니다. DRP를 활용하여 deep과 hand crafted prior를 포함한 불확실성을 고려한 비지도 학습 이미지 디블러 모델을 제안했습니다.

보다 구체적인 정보를 위해서는 논문 링크를 참고해주세요. 다음은 논문 링크입니다.

[CVPR 2023 open access](https://openaccess.thecvf.com/content/CVPR2023/html/Tang_Uncertainty-Aware_Unsupervised_Image_Deblurring_With_Deep_Residual_Prior_CVPR_2023_paper.html){: target="_blank"}