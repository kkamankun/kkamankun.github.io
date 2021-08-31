---
title: "[산학연계 SW프로젝트] 코드 리뷰"
categories:
    - Image Inpainting
# layout: default
---
용어
---

netG: refinement generator

netP: rough generator

netD: discriminator

netF: feature discriminator

Pytorch 이해
---

forward() 함수:  model이 학습데이터를 입력받아서 forward 연산을 진행시키는 함수. model이란 이름의 객체를  생성 후, model(입력 데이터)와 같은 형식으로 객체를 호출하면 자동으로 forward 연산이 수행됨.

torch.nn: 신경망을 구축하기 위한 다양한 데이터구조, 레이어가 정의

torch.optim: SGD 등의 파라미터 최적화 알고리즘

torch.utils.data: gradient descent 계열의 반복 연산을 할 때 사용하는 미니배치용 유틸리티 함수 포함

Variable: Pytorch Tensor와 동일한 API 제공. Pytorch autograd는 자동으로 변화도를 계산할 수 있음. 인자인 requires_grad = False이면 변화도를 계산할 필요가 없고 True이면 계산할 필요가 있다. 

detach(): 기존 tensor에서 gradient 전파가 안되는 텐서 생성. clone()과 더불어 기존 tensor를 복사하는 방법

torch.zeros_like(): 입력 텐서와 크기를 동일하게 하면서 값을 0으로 채운 텐서를 생성

torch.numel(): 입력 텐서의 원소의 개수를 반환

torch.narrow(input, dim. start, length): 입력 텐서의 축소 버전인 새로운 텐서를 반환

torch.squeeze(): 차원이 1인 차원을 제거

unfold(): tensor의 dimension size들을 조절하여 원하는 텐서를 얻을 수 있음

torch.index_select(): 텐서 중에 특정 값만 조회

머신러닝 이해
---

batch size: 한 번의 batch 마다 주는 데이터 샘플의 size. 여기서 batch는 나눠진 데이터셋을 뜻함.

파이썬 이해
---

functools.partial(): 기존의 함수와 구현은 동일하고 파라미터만 미리 정해준 또 다른 함수를 생성