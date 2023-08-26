---
title: "Image Data Augmentation for Deep Learning: A Survey (arXiv 2022)"
categories:
    - Paper Review
# layout: default
---
Image data augmentation은 딥러닝 모델의 상용화를 위해 학습 데이터의 양과 다양성을 늘려줄 수 있는 중요하고 필수적인 방법입니다. 이 방법은 크게 basic과 advanced 접근법으로 구분할 수 있습니다.

![Taxonomy](https://github.com/developerTae/developerTae.github.io/assets/46318721/fe34cc31-c04d-4c37-806b-bb20a12874db)

첫 번째, basic 접근법에는 image manipulation, image erasing, image mix가 있습니다. Image manipulation은 rotation, flipping, cropping 등과 같은 image 변형과 관련된 기법입니다. Image erasing은 image 내부의 하나 또는 그 이상의 지역을 지우는 기법입니다. Image mix는 두 개 또는 그 이상의 전체 image나 일부 지역을 혼합하는 기법입니다.

![Image Manipulation](https://github.com/developerTae/developerTae.github.io/assets/46318721/c8746890-7fa0-43d0-a903-7e85ec20b02d){: width="70%"",height="70%""}

두 번째, advanced 접근법에는 auto augment, feature augmentation, deep generative models가 있습니다. 더 향상된 성능을 얻을 수 있는 augmentation 방법을 자동으로 찾는 기법입니다. Augmentation은 노이즈가 있고 추론에 부정적인 영향을 끼칠 수 있다는 단점이 있습니다. 입력 공간에만 augmentation을 적용하던 것을 넘어서, 학습된 feature 공간에 변형을 수행하는 feature augmentation이 있습니다. Data augmentation의 목표는 분포로부터 샘플들을 그려내는 것입니다.

다양한 augmentation 기법들이 성능 향상에 도움이 되지만, 아직 극복해야 하는 문제들이 존재합니다. 먼저, 이 기법에 관한 이론적 연구가 부족하다는 것입니다. 이 기법이 성능 향상에 어떻게 도움이 되는지 이해가 부족합니다 (e.g. pairing samples와 mix up). 그리고 충분한 데이터셋 양에 대한 이론이 없습니다. 또한, 표준 평가 방법이 정해져 있지 않습니다. 그래서 어떻게 평가를 할 것인지가 문제입니다.  이 외에도 클래스 불균형 문제가 있습니다. 그리고 생성된 데이터의 양에 관련된 문제가 있습니다. 학습 데이터의 양을 증가해도 성능 향상으로 이어지지 않는 경우가 있습니다. 마지막으로 augmentation 기법의 선택과 조합에 관한 문제입니다. 새로운 데이터를 만들기 위해 다양한 augmentation 기법들을 어떻게 조합할 것인지가 중요한 문제입니다.

보다 구체적인 정보를 위해서는 논문 링크를 참고해주세요. 다음은 논문 링크입니다.

[arXiv](https://arxiv.org/abs/2204.08610){: target="_blank"}