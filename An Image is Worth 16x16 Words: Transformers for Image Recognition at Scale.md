## An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale
- https://arxiv.org/abs/2010.11929
<img width="50%" alt="image" src="https://user-images.githubusercontent.com/41967014/173275878-f2ad04d2-05e1-4e32-957d-b621b5c4fc4e.png">

********
### Summary
- `Computer Vision` 분야에 기존 `NLP`의 강자인 `Transformer` 모델을 접목시켜 성공한 사례
- 기존에는 `Computer Vision` 분야에서는 `CNN`과 `Attention이` 혼합되어 사용되었지만, `Attention`만 순수하게 사용
- 좋은 성능을 내기 위해서는 `Large Dataset`에 대한 `Supervised Pre-Training`이 필요함

********
### Introduction
<img width="240" alt="image" src="https://user-images.githubusercontent.com/41967014/173277382-e86e3bcc-d817-47ba-aad9-b72f9597b641.png">

- 이미지에 시퀀스 모델인 `Transformer`를 도입시키기 위해서 `Patch` 개념을 추가
- 하나의 이미지에 대해서 작은 `Patch`로 분할하고 이 `Patch`들의 `Linear Embedding`의 시퀀스를 `Transformer`의 `Input`으로 전달
- 이미지의 `Patch`는 `v`에서 `Token`과 같은 개념의 역할
- mageNet과 같은 mid-size 데이터셋에 대해서의 결과와 비슷한 사이즈의 ResNet-like architecture 모델들과 비교했을 때 정확도가 떨어짐
- 데이터셋이 충분치 않으면 Generalize를 위한 모델의 고유 가정인 Inductive Biases로써 CNN 학습시에 모델에 내재되어 있는 Translation Equivariance나 Locality와 같은 부분이 Transformer에 부족하기 때문이라고 판단
- 그러나 데이터셋이 커지면 충분히 좋은 성능을 보이는데 이는 Large Dataset Learning이 기존 CNN 계열 모델의 Inductive Bias를 능가할 수 있다는 것을 확인
- 즉, ViT로 충분히 큰 데이터셋에 대해서 Pre-Training을 진행하고 작은 데이터셋에 대하여 fine-tuning을 진행하게 되면 SoTA를 달성할 수 

********
### Related Work



********
### Method


********
### Experiments


********
### Conclusion
