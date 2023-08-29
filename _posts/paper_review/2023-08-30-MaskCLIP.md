---
title: "MaskCLIP: Masked Self-Distillation Advances Contrastive Language-Image Pretraining (CVPR 2023)"
categories:
    - Paper Review
# layout: default
use_math: true
---
본 논문은 masked self-distillation을 CLIP (Contrastive language image pretraining)에 적용한 새로운 기법을 제안합니다. Self-knowledge distillation은 모델 그 자체에서 지식을 증류하여 학습에 스스로 활용하는 기법으로, masked self-distillation은 전체 이미지에 대한 representation을 masking된 이미지에 대한 representation으로 증류하는 것입니다. 이 기법은 semantic한 local patch representation 학습에 이점이 있습니다.

Student와 teacher model은 동일한 구조를 갖고 있으며, knowledge는 full image로부터 masked image로 증류됩니다. 본 논문에서는 제안하는 masked self-distillation의 효과를 다양한 실험을 통해 증명했습니다.

![MaskCLIP](https://github.com/kkamankun/kkamankun.github.io/assets/46318721/faa4dd73-d5e9-452e-8e27-7ee2cc7979b1)

MaskCLIP의 $E_I$는 Vision Transformer (ViT)를 사용합니다. Visual encoder의 학습을 위해 외부의 teacher를 가져오는 것이 아니라, student와 같은 구조를 갖는 mean teacher model을 사용하여 self-distillation을 수행합니다. Teacher의 parameters는 student로부터 exponential moving averages (EMA) 입니다. 

$\alpha$는 smoothing updates를 위한 hyper-parameter입니다. 먼저 input image $I$가 EMA model $\bar E_I$ (teacher model)에 입력되고, 랜덤하게 masking 한 뒤에 $E_I$ (student model)에 입력합니다. Masking은 input image의 75% 비중으로 설정합니다.

Masked image modeling을 통해 local patch representation을 학습할 수 있습니다. 본 논문에서 제안하는 masked self-distillation을 통해 더 semantic 한 local patch representation을 학습할 수 있다고 합니다.

정리하면, 본 논문에서는 masked self-distillation을 통해 semantic 한 local patch representation을 학습할 수 있는 MaskCLIP 구조를 제안했습니다.

보다 구체적인 정보를 위해서는 논문 링크를 참고해주세요. 다음은 논문 링크입니다.

[CVPR 2023 open access](https://openaccess.thecvf.com/content/CVPR2023/html/Dong_MaskCLIP_Masked_Self-Distillation_Advances_Contrastive_Language-Image_Pretraining_CVPR_2023_paper.html){: target="_blank"}