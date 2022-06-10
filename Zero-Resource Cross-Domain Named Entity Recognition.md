# Zero-Resource Cross-Domain Named Entity Recognition
<img width="33%" alt="image" src="https://user-images.githubusercontent.com/41967014/172985907-d548606d-046d-4509-97a6-0eb6252295b3.png">

*******
### Summary


*******
### Model Architecture
![image](https://user-images.githubusercontent.com/41967014/172986136-f6904005-0628-44bb-a66a-2ccb3ee3a269.png)
- 모델 구조는 MTL과 MoEE 모듈을 결합한 BiLSTM-CRF 모델


*******
### Mixture of Entity Experts


*******
### Optimization
![image](https://user-images.githubusercontent.com/41967014/172987002-f212e698-3de6-4a0b-8689-012e9969fbdd.png)
- 학습과정중에 $Task_1$, $Task_2$ and $Gate$에 대하여 cross-entropy를 활용함.
- $J$는 학습 데이터의 수, $|Y_j|$는 토큰의 길이, $p_{jk}$와 $y_{jk}$는 각각의 토큰에 대한 예측과 레이브, 첨자는 각각의 Task를 의미
- 최종 목적 함수는 앞서 언급한 모든 손실 함수의 합을 최소화 하는 것
