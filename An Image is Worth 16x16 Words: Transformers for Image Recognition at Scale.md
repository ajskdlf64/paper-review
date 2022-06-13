# An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale
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
- `mageNet`과 같은 `mid-size` 데이터셋에 대해서의 결과와 비슷한 사이즈의 `ResNet-like architecture` 모델들과 비교했을 때 정확도가 떨어짐
- 데이터셋이 충분치 않으면 Generalize를 위한 모델의 고유 가정인 Inductive Biases로써 CNN 학습시에 모델에 내재되어 있는 `Translation Equivariance`나 `Locality`와 같은 부분이 `Transformer`에 부족하기 때문이라고 판단
- 그러나 데이터셋이 커지면 충분히 좋은 성능을 보이는데 이는 `Large Dataset Learning`이 기존 `CNN` 계열 모델의 `Inductive Bias`를 능가할 수 있다는 것을 확인
- 즉, `ViT`로 충분히 큰 데이터셋에 대해서 `Pre-Training`을 진행하고 작은 데이터셋에 대하여 `fine-tuning`을 진행하게 되면 `SoTA`를 달성할 수 있음

********
### Related Work
- 이미지에 대한 `self-attention`의 적용은 각각의 `픽셀`들이 `Token` 개념으로 하나의 픽셀에 대해서 다른 모든 픽셀에 대한 `Attention`을 계산해야 함
- 이는 픽셀 수에 따라서 `Quadratic`한 계산 복잡도를 가지며, Input 이미지의 화질이 좋아질수록 기하급수적으로 계산량이 늘어남
- 이를 해결하기 위해 몇가지 `Approximation Method`들이 시도됨
  - local self-attention, sparse attention, applying it in blocks of varying size ...
- 위의 방법들을 사용하면 어느 정도 성능은 보장되나 하드웨어를 효율적으로 사용하기 위해 `복잡한 엔지니어링` 기술이 필요함 

********
### Method
<img width="100%" alt="image" src="https://user-images.githubusercontent.com/41967014/173279449-27a97f45-4477-4970-8325-3a047f35c829.png">

- ㅇㅇㅇ ㅇㅇㅇ

**ViT : Vision Transformer**
- ㅇㅇㅇ ㅇㅇㅇ
- ㅇㅇㅇ ㅇㅇㅇㅇ

**Hybrid Architecture**
- ㅇㅇㅇ ㅇㅇㅇ
- ㅇㅇㅇ ㅇㅇㅇㅇ

**Fine-Tuning and Higher Resolution**
- ㅇㅇㅇ ㅇㅇㅇㅇ
- ㅇㅇㅇ ㅇㅇㅇㅇㅇ

********
### Experiments
**Setup**
- Datasets
  - ㅇㅇㅇ ㅇㅇㅇ
- Model Variants
  - ㅇㅇㅇ ㅇㅇㅇㅇ
- Training & Fine-Tuning
  - ㅇㅇㅇ ㅇㅇㅇㅇ
- Metrics
  - ㅇㅇㅇ ㅇㅇㅇㅇ

**Comparison to State of the Art**
- ㅇㅇㅇ ㅇㅇㅇㅇ
- ㅇㅇㅇ ㅇㅇㅇㅇㅇ

**Pre-Training Data Requirements**
- ㅇㅇㅇ ㅇㅇㅇㅇ
- ㅇㅇㅇ ㅇㅇㅇㅇㅇ

**Scaling Study**
- ㅇㅇㅇ ㅇㅇㅇㅇ
- ㅇㅇㅇ ㅇㅇㅇㅇㅇ

**Inspecting Vision Transformer**
- ㅇㅇㅇ ㅇㅇㅇㅇ
- ㅇㅇㅇ ㅇㅇㅇㅇㅇ

**Self-Supervision**
- ㅇㅇㅇ ㅇㅇㅇㅇ
- ㅇㅇㅇ ㅇㅇㅇㅇㅇ

********
### Conclusion
**Contribution**
- `Computer Vision` 분야에 `Transformer` 모델을 성공적으로 도입시킴
- 이전에 `Self-Attention`을 `Vision`에 접목시킨 연구들과는 다르게 이미지에 특화된 어떤 `Inductive Bias`를 추가하지 않음
- 이미지를 `Patch` 단위로 쪼개고 그 `Patch`를 `Token` 개념으로 사용
- 대용량 데이터셋을 이용해서 `Pre-Training`하면 성능이 매우 좋음

**Future research**
- `Image Detection`이나 `Image Segmentation` 등의 `Task` 확장
- `Self-Supervised Pre-Training` 방법의 최적화
- 성능 향상을 위한 `ViT`의 업그레이드
