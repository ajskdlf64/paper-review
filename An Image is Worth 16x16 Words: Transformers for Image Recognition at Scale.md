# An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale

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
