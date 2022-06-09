# Zero-Resource Cross-Lingual Named Entity Recognition

<img width="33%" alt="image" src="https://user-images.githubusercontent.com/41967014/172783205-663af5e1-53b9-4584-8dcb-33a9d5e1d32b.png">

*******
### Summary
- source 언어로 NER 모델을 학습한 후 unsupervised인 target 언어의 NER 모델로 transfer하자!
- 영어 태깅 데이터셋으로 NER 모델 학습 -> 영어-한국어 매칭 매트릭스 학습 -> augmented fine-tuning
  - 기존의 Bi-LSTM + CRF 방법으로 영어 NER 모델을 학습
  - "I"라는 영어 단어와 "나"라는 한글 단어가 매칭되는 Matrix 학습하기
  - 태깅된 source 데이터셋을 태깅된 target 데이터셋으로 변환하여 fine-tuning
- 5개의 언어에 대해서 실험 진행(English, Spanish, Dutch, German, Arabic, Finnish)
> KEY POINT : "어떻게 source-target 간의 matrix를 찾는가?" and "target 데이터에 대하여 labeled dataset을 어떻게 만들어 내는가?"

*******
### Source Base Model
![image](https://user-images.githubusercontent.com/41967014/172803378-0d33c383-78d2-4423-a45d-c09461de3980.png)

*******
### Cross-Lingual Model

*******
### Augmented Fine-Tuning
- word-level adversarial training은 단어를 독립적으로 매핑함.
- 그러나 NER은 Sequence Labeling task이고, word order는 각 언어에 따라 다르다.
- word-level corss-lingual mapping은 NER Tagging 정보를 고려하지 않고 단순히 word translation model이다.
![image](https://user-images.githubusercontent.com/41967014/172858632-e191a3ec-e70d-40ab-b8ac-6f688f824f59.png)
- 위의 그림처럼 각 word가 translation이 되지만, Entity별로 Clustering은 되지 않음.
- target encoder에서 target language ordering information과 source model의 NER Knowledge을 transfer하는 것을 동시에 배우기 위해서 새로운 학습법인 `augmented fine-tuning method`를 제시함.
  - source model pre-training through weight sharing
  - generating psudo target labels
  - joint training with feature augmentation
- Example
  - 
