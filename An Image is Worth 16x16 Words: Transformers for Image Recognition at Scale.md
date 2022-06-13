# An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale
- https://arxiv.org/abs/2010.11929
<img width="50%" alt="image" src="https://user-images.githubusercontent.com/41967014/173275878-f2ad04d2-05e1-4e32-957d-b621b5c4fc4e.png">

********
### Summary
![vit](https://user-images.githubusercontent.com/41967014/173297655-78584f26-7cab-4e72-9c7f-0c1462189a07.gif)
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
  - Pre-Trained
    - ImageNet / 1k classes and 1.3M images
    - ImageNet-21k / 21k classes and 14M images
    - JFT / 18 classes and 303M high-resolution images
  - Fine-Tuning
    - ImagetNet holdout set / 
    - ImagetNet Real / 
    - CIFAR-10 / 
    - CIFAR-100 / 
    - Oxford-IIIT Pets / 
    - Oxford Flowers-102 / 
    - 19-task VTAB / 각 task마다 1,000개의 train dataset으로 학습하고 평가
- Model Variants
  - <img width="50%" alt="image" src="https://user-images.githubusercontent.com/41967014/173298808-2a636c37-630d-46b2-bbd1-c4ba2b982e8f.png">
  - 다양한 사이즈의 모델을 생성
  - Base와 Large는 BERT 논문에 공개된 모델과 완전 동일
  - 더 큰 규모의 Huge 모델을 추가
  - ViT-L/16의 의미는 ViT-Large 모델에 16x16 패치를 적용한 모델
  - 비교 모델로 Baseline CNN 모델은 RwesnNet을 사용. 이 때 Batch Normalization을 Group Normalization으로 교체하고 Standard Convolution을 사용 이런 모델을 ResNet(BiT)로 표기
- Training & Fine-Tuning
  - 옵티마이저는 Adam을 사용
  - $\beta_1 = 0.9$, $\beta_2 = 0.999$, $batch-size = 4,096$
  - Weight Decay 적용 -> 모델의 tranfer learning시 도움을 줌
  - Learning Schedule : linear learning rate warmup and decay
  - Fine-Tuning할 때는 SGD with momentum 옵티마이저를 사용, $batch-size = 512$
- Metrics
  - 모델의 평가는 Downstream Task에 대하여 few-shot accuracy or fine-tuning accuracy로 평가를 진행

**Comparison to State of the Art**
- <img width="406" alt="image" src="https://user-images.githubusercontent.com/41967014/173300666-b4c0a2fe-35db-4c16-9a1d-e1575ff8f05d.png">
- <img width="400" alt="image" src="https://user-images.githubusercontent.com/41967014/173300863-c6b8d29d-e939-4113-9070-c4a7ba6949f5.png">
- <img width="399" alt="image" src="https://user-images.githubusercontent.com/41967014/173300897-dda420a4-68be-4f1f-9172-9cdf5734d76f.png">

- ㅇㅇㅇ ㅇㅇㅇㅇ
- ㅇㅇㅇ ㅇㅇㅇㅇㅇ

**Pre-Training Data Requirements**
- ㅇㅇㅇ ㅇㅇㅇㅇ
- ㅇㅇㅇ ㅇㅇㅇㅇㅇ

**Scaling Study**
- <img width="404" alt="image" src="https://user-images.githubusercontent.com/41967014/173300917-84e33ef8-ca71-4cc1-b276-50e874ae317e.png">
- `JFT-300M` 데이터셋으로 `transfer learning`의 성능 평가를 수행하는데 각각의 모델의 `performance vs pre-training cost`를 평가함
- ResNet 계열 7개의 모델, ViT 계열 6개의 모델, Hybrid 계열 모델 5개를 고랴함
- ViT 계열(파란점) 모델들이 ResNet 계열(회색점)보다 성능이 좋음
- 동일 성능을 위해서 ResNet 계열 모델들이 2배 ~ 4배 가량의 cost가 더 필요

**Inspecting Vision Transformer**
- <img width="403" alt="image" src="https://user-images.githubusercontent.com/41967014/173300984-d42cc0a7-1004-4153-b1de-d7d150ce82da.png">
- ㅇㅇㅇ ㅇㅇㅇㅇ
- ㅇㅇㅇ ㅇㅇㅇㅇㅇ

**Self-Supervision**
- `Transformer` 기반 모델들은 `NLP task`에서 인상적인 포퍼먼스를 보여줌
- 그러나 핵심은 대규모의 데이터셋을 활용한 `self-supervised pre-training`으로부터 비롯됨
- 본 연구에서는 `BERT`의 `pre-training task` 중 하나인 `masked language modeling`을 모방하여 `masked patch prediction for self-supervision`을 실험해보았고, 그 결과 유의미한 성능 향상을 발견함
- 그러나 여전히 `supervised pre-training`보다는 많이 못미치는 성능

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
