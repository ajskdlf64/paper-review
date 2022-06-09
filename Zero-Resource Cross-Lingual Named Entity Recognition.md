# Zero-Resource Cross-Lingual Named Entity Recognition

<img width="33%" alt="image" src="https://user-images.githubusercontent.com/41967014/172783205-663af5e1-53b9-4584-8dcb-33a9d5e1d32b.png">

### 요약
- source 언어로 NER 모델을 학습한 후 unsupervised인 target 언어의 NER 모델로 transfer하자!
- 영어 태깅 데이터셋으로 NER 모델 학습 -> 영어-한국어 매칭 매트릭스 학습 -> augmented fine-tuning
  - 기존의 Bi-LSTM + CRF 방법으로 영어 NER 모델을 학습
  - "I"라는 영어 단어와 "나"라는 한글 단어가 매칭되는 Matrix 학습하기
  - 태깅된 source 데이터셋을 태깅된 target 데이터셋으로 변환하여 fine-tuning
- 5개의 언어에 대해서 실험 진행(English, Spanish, Dutch, German, Arabic, Finnish)
> 두가지 키포인트 : "어떻게 source-target 간의 matrix를 찾는가?" and "target 데이터에 대하여 labeled dataset을 어떻게 만들어 내는가?"

### Source Base Model
![image](https://user-images.githubusercontent.com/41967014/172803378-0d33c383-78d2-4423-a45d-c09461de3980.png)


### Cross-Lingual Model





















## Abstact
> Recently, neural methods have achieved state-of-the-art (SOTA) results in Named Entity Recognition (NER) tasks for many languages without the need for manually crafted features. However, these models still require manually annotated training data, which is not available for many languages. In this paper, we propose an unsupervised cross-lingual NER model that can transfer NER knowledge from one language to another in a completely unsupervised way without relying on any bilingual dictionary or parallel data. Our model achieves this through word-level adversarial learning and augmented fine-tuning with parameter sharing and feature augmentation. Experiments on five different languages demonstrate the effectiveness of our approach, outperforming existing models by a good margin and setting a new SOTA for each language pair.

최근들어 Neural Method들이 `manually crafted features` 필요 없이 수 많은 언어들의 NER task에서 SoTA를 기록하고 있다.  그러나 이러한 모델을은 여전히 `manually crafted features` 데이터를 필요로하고, 이는 대부분의 언어에서는 가능하지 않다. 따라서 본 연구에서는 `unsupervised cross-lingual NER model`을 제안한다. 이를 통해 한가지 언어로부터 NER Knowledge을 다른 언어로 transfer 할 수 있게 한다.

## Problem Definition
- 소스 언어(Source)의 NER 지식을 타겟 언어(Target)로 Unsupervised Transfer Learning 하는 방법 (ex. 영어로 NER 모델을 만든 다음 한국어 NER 모델로 변환)
- 문제 정의에 필요한 가정
  - source and target 사이에 mono-lingual corpora에 접근할 수 있어서 FastText와 같은 Pre-Trained word embedding을 만들 수 있다.
  - Source Language Dataset에 포함된 Entity에 대해서만 Training 가능하다.
  - model selection을 위한 2가지 Validation 시나리오를 고려한다.
    - labeled target language validation set
    - only source language validation set
- cross-lingul model을 학습하는 것은 2가지 fundamentatl step과 관련이 있다.
  - source와 target 사이의 mapping을 학습하는 것
  - task objective를 최대화하도록 매핑된 resources을 재학습하는 것
  
## Base model
![image](https://user-images.githubusercontent.com/41967014/172803378-0d33c383-78d2-4423-a45d-c09461de3980.png)
- 입력 문장이 들어옴. $s = (w_1, w_2, ..., w_n)$
- 먼저, 각 토큰($w_k$)에 대하여 chracter-level Bi-LSTM으로 인코딩

## Cross-lingual Model
- 목표는 source와 target 언어 사이의 NER Entity 분포들의 매핑을 학습하는 것
- NER에 대한 Neural Method들은 fixed or contextualized pre-trained embeddings에 과하게 의존함.
- 그러나 2개의 서로 다른 언어들에 대하여 각각 embedding을 학습할 때, 그들의 distribution spaces는 관련된 언어들이라고 해도 전혀 다른 양상을 갖는다.
![image](https://user-images.githubusercontent.com/41967014/172836147-2f9fac3f-c66d-4cf3-919b-0fd9de4b1e9a.png)
- Figure3에서 (a)를 보면, 영어와 스페인어에 대한  t-SNE plot인데 분포가 완전 다르다.
- (b)는 adversarial training을 적용한 경우, (c)는 our common encoder를 적용한 경우.
- 결국 Cross-Lingual Model은 (a)의 두 언어에 대해서 NER 태깅 정보를 반영하면서 (c)의 형태로 학습하는 매우 챌린징한 테스크.
- 본 연구에서 3가지 새로운 요소를 base model에 추가.
  - a separate encoder for the target language with shared character embeddings (box on the right) followed by a target-specific dense layer,
  - wordlevel adversarial mappers that can map word embeddings from one language to another (shown in the middle of the two boxes)
  - an augmented fine-tuning method with parameter sharing and feature augmentation.

### target encoder with shared character embedding
### word-level adversarial mapping
### augmented fine-tuning

## Experiments
### Dataset
- English : 14,041 / 3,250 / 3,453
- Spanish : 8,323 / 1,915 / 1,517 
- Dutch : 15,519 / 2,821 / 5,076
- German : 12,152 / 2,827 / 3,005
- Arabic : 2,166 / 267 / 254
- Finish : 13,497 / 986 / 3,512
### Compared Models
- **Source-Mono**
   - We train an NER model on the source language with source word embeddings and apply it to the target language with target embeddings, which can be pre-trained or randomly initialized. This model does not use any cross-lingual information.
   - source word embedding으로 NER 모델을 학습하고 pre-trained or randomly initialized에 사용, 이 모델은 언어 간 정보는 사용 X
- **Cross-Word**
   - We project source and target word embeddings to a common space using the unsupervised mapper ($W_{s->t} or W_{t->s}$). This model uses word-level crosslingual information learned from adversarial training and the Procrustes-CSLS refinement procedure.
   - unsupervised mapper를 아용하여 source & target word embedding에 투영, adversal training과 procrustes-CSLS refinement를 통해 cross-lingual information을 학습)
- **Cross-Shared**
   - This model is the same as Cross-Word, but the weights of the forward and backward LSTM cells are shared to encourage order invariance in the model.
   - 이 모델은 cross-word와 동일하지만 모델의 order invariancce을 위해서 LSTM의 가중치를 공유함
- **Cross-Augmented**
   - This is our full cross-lingual model trained with source labels and target pseudo-labels generated by the pretrained model and the model itself.
   - pre-trained 모델과 모델 자체에서 생성된 source label과 target pesudo-label로 훈련된 full cross-lingual 모델
### Results
![image](https://user-images.githubusercontent.com/41967014/172809017-db2e0ad2-49cb-475a-acc8-edc6ce093f03.png)
![image](https://user-images.githubusercontent.com/41967014/172809063-48fe4e93-a155-48a4-8ffc-5eeaead5a77d.png)
![image](https://user-images.githubusercontent.com/41967014/172809309-07b9f9c0-7481-4dd8-8bdc-46dd3e882f6c.png)
![image](https://user-images.githubusercontent.com/41967014/172809368-5ac5fccf-57ab-4f7d-9766-5fa625cf049d.png)




