# Zero-Resource Cross-Domain Named Entity Recognition
<img width="33%" alt="image" src="https://user-images.githubusercontent.com/41967014/172985907-d548606d-046d-4509-97a6-0eb6252295b3.png">

*******
### Summary
- 여러 도메인을 처리할 수 있는 NER 모델을 만들자
- MoE의 Expert 개념을 도입해서 Entity Experts를 도입 
- 일반적인 BiLSTM-CRF를 활용한 모델에 MoE와 MTL 모듈을 결합

*******
### Model Architecture
![image](https://user-images.githubusercontent.com/41967014/172986136-f6904005-0628-44bb-a66a-2ccb3ee3a269.png)
- 모델 구조는 MTL과 MoEE 모듈을 결합한 BiLSTM-CRF 모델
- 그림 (a)의 왼쪽은 일반적인 BiLSTM-CRF를 결합한 모델
- 보통은 NER에서 softmax 값을 통해 어떤 entity로 쓰였는지 판별하는데, 여기서는 여러 도메인에 적용하기 위해 바로 예측하는게 아니라 일단 사전에 정의된 엔티티인지 아닌지를 먼저 판별
- 그 다음 어떤 엔티티인지에 대해서는 MoE 모듈을 이용해서 confidence 값으로 구체적인 엔티티를 예측
- MoE 모듈에서 Expert는 늘리고 줄이고 가능하기에 few shot 으로 $Task_2$의 엔티티를 금방 만들 수 있나?

*******
### Mixture of Entity Experts
<img width="396" alt="image" src="https://user-images.githubusercontent.com/41967014/173003665-9934314d-9700-40a4-a5d5-d54493c19696.png">

- 전통적인 NER 모델은 features of the tokens and the context에 기반하여 예측함
- 다른 entity 간의 confusion으로 인해 domain에 쉽게 overfit되고 generalization이 어려움
- 따라서 위의 그림의 (b)의 MoEE 프레임워크를 이용해서 이를 해결
- MoEE가 생성한 featrue과 기존 feature를 결합하여 예측을 진행
- 각각의 Entity Experts는 linear layer 구조를 갖는다
- Entity가 아닌 것은 하나의 특수 Entity로 간주한다
- expert gate는 linear layer + softmax layer를 결합한 모양임. 이를 통해 entity experts의 confidence distribution을 생성함
- 이제 전이시킬 $Task_2$의 gold label을 이용해서 expert gate를 학습시킴
- 마지막으로 meta-expert를 통해서 모든 experts들의 feature를 통합함

*******
### Optimization
![image](https://user-images.githubusercontent.com/41967014/172987002-f212e698-3de6-4a0b-8689-012e9969fbdd.png)
- 학습과정중에 $Task_1$, $Task_2$ and $Gate$에 대하여 cross-entropy를 활용함.
- $J$는 학습 데이터의 수, $|Y_j|$는 토큰의 길이, $p_{jk}$와 $y_{jk}$는 각각의 토큰에 대한 예측과 레이브, 첨자는 각각의 Task를 의미
- 최종 목적 함수는 앞서 언급한 모든 손실 함수의 합을 최소화 하는 것

*******
### Results
![image](https://user-images.githubusercontent.com/41967014/172987792-b1701aaa-d5fa-4d17-9811-7dd5d5bd2c81.png)
- 문장이 들어오면, 각각의 Entity Expert에 대해서 각각의 토큰들에 대해서 confidence score를 계산
- "Drudge"의 경우 `PER`과 `ORG`가 헷갈릴 수 있음
- “Drudge”, the expert gate gives high confidence on more than one expert (e.g., “PER” and “ORG”) since the model is not sure whether “Drudge” is a “PER” or “ORG”. Our model is expected to learn the “PER” and “ORG” expert representations based on the hidden state of “Drudge”, which contains the information of this token and its context, and then combine the expert represen- tations for the prediction.
