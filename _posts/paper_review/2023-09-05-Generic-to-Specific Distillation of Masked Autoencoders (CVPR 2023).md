---
title: "Generic-to-Specific Distillation of Masked Autoencoders (CVPR 2023)"
categories:
    - Paper Review
# layout: default
use_math: true
---
Vision transformers (ViTs)는 self-supervised learning 방식으로 대량의 데이터셋을 학습했을 때, 높은 성능으로 주목받았습니다. Pixels, tokens, features 재구성을 학습하는 masked image modeling (MIM)은 large ViT 모델의 성능을 더 끌어올렸습니다.

그러나, small ViT 모델은 self-supervised learning이나 대량의 데이터셋이 주는 장점을 잘 살리지 못했습니다. 따라서, 이 모델은 CNN 모델보다 종종 낮은 성능을 나타냈습니다.

본 논문에서는 generic-to-specific distillation (G2SD)을 통해 lightweight ViT가 teacher로부터 task-agnostic과 task-specific representations를 전달받아, 더 나은 성능을 나타내도록 했습니다. 이때, teacher는 masked autoencoders입니다. Generic과 specific distillation에서 각각 task-agnostic, task-specific features를 전달합니다.

![Generic_to_Specific](https://github.com/kkamankun/kkamankun.github.io/assets/46318721/dd173074-7e5e-486e-a885-145fe68eb771)

Generic distillation에서는 large pre-trained masked autoencoders의 task-agnostic knowledge를 전달합니다. 아래의 수식은 generic distillation loss입니다.

\[L_{GD}=\sum_{i\epsilon \left \lbrace V \cup M \right \rbrace}Smooth-\ell_{1}\left ( LN(\hat{z}_{i}^{t}) - z_i^{s} \right )\]

$L_{GD}$를 통해 visible tokens $V$의 feature extraction 방법을 학습합니다. 또한, 이 loss를 통해 masked tokens $M$에 대해 context 모델링 방법을 학습합니다.

Specific distillation은 downstream tasks (e.g. image classification, object detection, semantic segmentation)에 최적화된 features를 전달합니다.

\[L_{SD}=L_{Task}\left ( f^{s}\left ( x \right ), Y \right )+\beta L_{KD}\left ( f^{s}\left ( x \right ), f^{t}\left ( x \right ) \right )\]

이때, teacher model $f^t$는 MAE 방법으로 사전 학습된 뒤에, 특정 task로 fine-tuned 되어있습니다. $L_{Task}$는 task loss function, $L_{KD}$는 task-specific distillation loss를 뜻합니다.

정리하면, 본 논문에서는 사전 학습된 large models로부터 lightweight ViTs로 generic-to-specific distillation을 통해 지식을 전달하는 방법을 제안합니다. 제안하는 방법을 통해 task-agnostic과 task-specific knowledge를 전달합니다.

보다 구체적인 정보를 위해서는 논문 링크를 참고해주세요. 다음은 논문 링크입니다.

[CVPR 2023 open access](https://openaccess.thecvf.com/content/CVPR2023/html/Huang_Generic-to-Specific_Distillation_of_Masked_Autoencoders_CVPR_2023_paper.html){: target="_blank"}