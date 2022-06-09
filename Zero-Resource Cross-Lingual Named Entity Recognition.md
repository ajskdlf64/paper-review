# Zero-Resource Cross-Lingual Named Entity Recognition

<img width="33%" alt="image" src="https://user-images.githubusercontent.com/41967014/172783205-663af5e1-53b9-4584-8dcb-33a9d5e1d32b.png">

*******
### Summary
- source 언어로 NER 모델을 학습한 후 unsupervised인 target 언어의 NER 모델로 transfer하자!
- 영어 태깅 데이터셋으로 NER 모델 학습 -> 영어-한국어 매칭 매트릭스 학습 -> augmented fine-tuning
  - 기존의 Bi-LSTM + CRF 방법으로 영어 NER 모델을 학습
  - "I"라는 영어 단어와 "나"라는 한글 단어가 매칭되는 Matrix 학습하기
  - 태깅된 source 데이터셋을 태깅된 target 데이터셋으로 변환하여 fine-tuning
- 알파벳을 공유하는 2개의 언어에 대해서 공유되는 character embedding과 각각의 학습된 word embedding을 이용
- 5개의 언어에 대해서 실험 진행(English, Spanish, Dutch, German, Arabic, Finnish)
> KEY POINT : "어떻게 source-target 간의 matrix를 찾는가?" and "target 데이터에 대하여 labeled dataset을 어떻게 만들어 내는가?"

*******
### Source Base Model
![image](https://user-images.githubusercontent.com/41967014/172803378-0d33c383-78d2-4423-a45d-c09461de3980.png)
1. char를 공유하는 2개의 언어에 대해서 가능(한국어-영어 X / 영어-스페인어 O) -> char embedding을 공유.
2. 각각의 언어별로 인코더를 만들고 공유하는 common encoder를 만듬.
   - 이 때, 독립된 인코더는 orthographic 속성들을 중점으로 학습(대문자, 접두사, 접미사 등)
   - 공유하는 인코더는 source 에서 target으로 변환시키는데 집중
3. source와 target에 대해서 각각의 word embedding이 존재
4. 이 때 2개의 word embedding의 파라미터 공유를 위해서 W Matrix를 두어 학습.

*******
### Cross-Lingual Model
![image](https://user-images.githubusercontent.com/41967014/172858632-e191a3ec-e70d-40ab-b8ac-6f688f824f59.png)
- 학습의 주요 목표는 source 와 target 언어 사이의 NER Entity 분포의 매핑을 배우는 것
- 즉, 위의 그림에서 (a)를 (c)로 만드는 것





*******
### Augmented Fine-Tuning
- "The capital city of Korea<LOC> is Seoul<LOC>." 을 이용해 source ner model을 학습. (이 때 word embedding and chracter embedding을 동시 사용.)
- "한국<LOC>의 수도는 서울<LOC>이다." 을 이용해 target ner model을 학습. 이 때 저렇게 완벽한 문장이 들어가는 것이 아니라 (학습된 word/char embedding을 통해 변환된 벡터값을 사용.)
- source 문장 학습 시, 만든 feature embedding을 target ner model에 활용.
- 번외로, 문장의 길이가 길어질수록 노이즈가 많이 낄텐데, stochastic selection method을 사용하여 해결.
  - 제안한 stochastic training 스케줄 길이는 모델이 short와 long 문장들 사이의 learning-inference gap을 해결할 수 있다.
