---
title: "[산학연계 SW프로젝트] 논문 번역"
categories:
    - Image Inpainting
# layout: default
---
논문
---
Hongyu Liu, et.al. "Coherent Semantic Attention for Image Inpainting" arXiv: 1905.12384 (2019)

Abstract
---

이전 딥러닝 기반 인페인팅 기법들은 성능이 대체로 좋지만, 지엽적인 픽셀 간의 불연속으로 인하여 흐릿함이나 왜곡 현상이 발생한다. 이는 missing regions에 대한 피처(feature) 연속성과 의미론적 상관성을 무시했기 때문이다. 따라서, 그림을 복원하는 사람의 행동을 적용한 생성 모델을 제안한다. 제안하는 모델에는 CSA layer가 있다. CSA layer는 missing rigions의 피처들 간의 의미론적 상관성을 따짐으로써 전후 구조를 잘 보존해주고 훼손된 영역에 대한 예측을 잘 수행한다.

모델은 rough와 refinement 두 단계로 구분된다. 각 네트워크는 U-Net 구조의 neural network이다. CSA layer는 refinement 단계의 encoder 안에 있다. 네트워크 학습 안정화와 CSA layer의 학습을 잘 해내기 위해서 consistency loss를 제안한다. Consistency loss는 CSA layer와 CSA layer에 대응하는 디코더의 layer가 VGG feature layer(ground truth image)에 가까워지도록 유도한다.

실험은 CelebA, Places2, Paris StreetView 데이터셋으로 수행했다.

Introduction
---

### Related Works

이미지 인페인팅은 missing region을 그럴듯한 추정을 통해 합성해내는 기법이다. 원하지 않는 객체를 제거하고, 가려진 영역을 복원하고, 손상된 부분을 복원하는데 이용된다. 이미지 인페인팅의 핵심은 의미론적인 구조를 전범위적으로 잘 보존하고 missing region에 대한 실제와 같은 텍스처를 잘 생성해내는 것이다.

이미지 인페인팅의 전통적인 기법에는 텍스처 합성이 있다. Barnes 등은 Patch-Match 알고리즘을 제안했다. Hole 주변에서 missing parts를 채워넣기 가장 적합한 패치를 찾는 과정을 반복하는 알고리즘이다. Wilczkowiak 등은 더 나아가 적합한 patch가 있음직한 탐색 영역을 탐지해내는 방법을 제한하였다. 하지만, 고차원적인 의미론적 이해가 부족하고 지역적으로 유일한 패턴을 재구성해내지 못하는 단점이 있다.

Deep convolutional neural networks를 통한 이미지 인페인팅 기법은 data의 분포를 학습하여 이미지의 의미론적인 분석을 해냄으로써 그럴듯한 성능을 도출하였다. 그러나, contextual information을 이용하여 hole을 채워넣는데 실패하였고 노이즈 패턴이 결과에 출력되기도 하였다.

최근에는 contextual information을 이용하여 더 좋은 결과를 얻는 연구들이 있는데 크게 2가지로 분류할 수 있다. 첫째는 공간 어텐션을 이용한 기법이다. Missing regions를 채울 때 주변 이미지의 특성들을 참조한다. Contextual information을 통해 생성된 내용들의 의미론적 일관성을 확보할 수 있다는 장점이 있다. 하지만 직사각형 모양의 hole에만 적용할 수 있고 픽셀 불연속과 갈라진 틈 같은 인공물이 나타나는 단점이 있다. 둘째는 유표한 픽셀에서 누락된 픽셀을 예측하는 기법이다. 불규칙한 hole에도 바르게 다룰 수 있다는 장점이 있다. 하지만 생성된 내용은 여전히 의미론적 결함이 존재하여 경계면에 인공물이 나타나는 단점이 있다. 이 방법들의 성능이 좋지 않은 이유는 생성된 내용 사이의 semantic relevance(의미적 관련성), feature continuity(피처 연속성)을 고려하지 않았기 때문이다. 이런 것들은 지역적 픽셀 연속성에 큰 영향을 끼치기 때문에 고려할 필요가 있다.

### 제안하는 모델

그림 복원하는 과정에서는 구상을 먼저 하여 고쳐나간다. 이를 통해 전범위적 구조의 일관성을 확보하고 지역적 픽셀의 연속성을 확보한다. CSA layer는 image feature map의 훼손된 부분을 채워넣는다. 각 unknown feature patch를 훼손되지 않은 영역에서 가장 유사한 패치로 초기화한다. 이후, 인접 패치들 간의 공간적 일관성을 고려하여 최적화하는 과정을 거친다. 이를 통하여, 전범위적인 의미론적 일관성을 먼저 확보하고 지역적 피처 일치성을 최적화 단계에서 확보한다. Consistency loss는 VGG feature layer와 CSA layer 사이의 거리를 측정하고 VGG feature layer와 CSA layer와 대응하는 디코더의 layer 사이의 거리를 측정한다. Feature patch discriminator를 제안하여 디테일을 개선했다. Feature patch를 사용하여 공식화가 더 간단해지고 학습이 더 안정적이게 되었다. Consistency loss 이외에도 reconstruction loss, relativistic average LS adversarial loss를 통해 모델이 더 잘 학습하도록 하였다. Hole regions의 deep 피처들 사이의 상관 관계를 고려하여 구성할 수 있는 CSA layer를 제안한다. CSA layer의 성능과 학습 안정성을 높여주기 위한 consistency loss를 제안한다. Consistency loss는 CSA layer와 CSA layer에 대응하는 디코더의 layer가 ground truth의 VGG features를 학습하도록 유도한다. 

Related Works
---

이미지 인페인팅 연구는 크게 2가지로 분류할 수 있다. Non-learning 기법과 learning 기법이다. Non-learning 기법은 diffusion-based와 patch-based로 저레벨 피처들을 다루는 기법들이 있다. Learning 기법들은 deep convolutional neural networks을 학습시켜서 추론한다. 그리고 어텐션 기반 이미지 인페인팅 기법들이 있다. Contextual region과 hole region 사이의 관계에 기반한 공간적 어텐션을 사용한다. 우선 대략적으로 예측해낸 패치들과 유사성 있는 배경 패치들을 찾는 것이다.