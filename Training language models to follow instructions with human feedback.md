# Training language models to follow instructions with human feedback

![image](https://user-images.githubusercontent.com/41967014/162145268-1ddacc39-8665-44f0-8ae5-ca74d5c6b65d.png)

Open AI / 2022.03 / Alignment / Computation and Language

![image](https://user-images.githubusercontent.com/41967014/163311757-13fba069-aa29-45b1-be7e-bbd0297ff24f.png)
![image](https://user-images.githubusercontent.com/41967014/163311909-b8acee43-d053-4054-acd2-f2bd8dc04dfc.png)

### Abstract
- 언어 모델을 크게 만든다고 해서 무조건 사용자의 의도를 더 잘 따르는 것은 아님.
- 종종 대형 모델들의 Output에는 untruthful(허위), toxic(독성) 등의 결과를 내뱉으며 이는 사용자에게 도움이 되지 않음. 즉 모델은 사람의 생각대로 결과를 보여주지 않음.
- 본 연구에서는 사람의 피드백 (Human Feedback)을 수용하여 사람의 의도를 모델의 결과에 반영할 수 있는 방법을 제안.
- 사람이 직접 작성한 프롬포트와 OpenAI API을 활용한 프롬포트를 이용하여 GPT-3를 fine-tuning할 Supervised Dataset을 구축함.
- 모델이 출력한 Output 순위에 대하여 인간의 피드백을 적용하고 강화학습을 사용하여 fine-tuning을 진행함.
- 그렇게 탄생된 모델을 InstructGPT라고 부름.
- 1.3B의 파라미터 수를 갖는 InstructGPT는 파라미터 수가 100배 많은 175B의 GPT-3 보다 성능이 좋음.
- InstructGPT는 공개되어 있는 NLP truthfulness and reduction in toxic 점수에서도 성능 저하를 최소화하면서도 좋은 결과를 보임.
- InstructGPT가 아직은 완벽하진 않지만, 본 연구의 결과가 fine-tuning 과정에서 인간의 의도와 모델의 의도를 일치시키는 한가지 유의미한 방법이라는 것을 보여줌.

### 1. Introduction
- 본 연구에서는 "fine-tuning approaches to aligning language models"에 초점을 맞춤.
- 인간의 피드백을 강화학습을 통해 모델에 반영하는 방법을 연구함.
- 모델의 결과에 대하여 인간의 선호도를 매기고 그것을 강화학습의 보상으로 이용하여 fine-tuning을 진행.
- 40명의 사람을 고용하여 데이터 레이블링 작업을 진행. 
- OpenAI의 API를 통해 제출된 프롬포트 중에 인간이 원하는 답변을 한 데이터셋과 프롬포트에 대하여 일부 라벨러가 직접 작성한 데이터셋을 이용하여 지도 학습 데이터셋을 구축함. 
- GPT-3의 행동을 "인간 가치(Human Value)"의 광범위한 이념 보다는 특정 그룹(연구자와 라벨러)의 명시된 선호도에 초점을 맞춤.
- 주요 연구 결과 : sizes (1.3B, 6B, and 175B parameters), and all of our models use the GPT-3 architecture. Our main findings are as follows:
  - Labelers significantly prefer InstructGPT outputs over outputs from GPT-3.
  - InstructGPT models show improvements in truthfulness over GPT-3.
  - InstructGPT shows small improvements in toxicity over GPT-3, but not bias.
  - We can minimize performance regressions on public NLP datasets by modifying our RLHF fine-tuning procedure.
  - Our models generalize to the preferences of “held-out” labelers that did not produce any train- ing data.
  - Public NLP datasets are not reflective of how our language models are used.
  - InstructGPT models show promising generalization to instructions outside of the RLHF fine- tuning distribution.
  - InstructGPT still makes simple mistakes.

### 2. Related Work
- 생략

### 3. Method and experimental details
![image](https://user-images.githubusercontent.com/41967014/163305614-05ba8c1e-b3da-4ccd-a76f-3d2064654c53.png)
- Method
  - Step1 : supervised fine-tuning (SFT)
  - Step2 : reward model (RM) training
  - Step3 : reinforcement learning via proximal policy optimization (PPO)
- Models
  - Supervised fine-tuning (SFT)
  - Reward modeling (RM)
  - Reinforcement learning (RL)

### 4. Results
  - ![image](https://user-images.githubusercontent.com/41967014/163308883-72d7f64f-e07f-4629-b107-e30e50ed1d1a.png)
  - ![image](https://user-images.githubusercontent.com/41967014/163308908-3cc06253-645e-4314-bccc-2e7b2ebf0f33.png)
  - ![image](https://user-images.githubusercontent.com/41967014/163308918-a1438b68-d19b-41c1-bb8a-bff4f9f11a48.png)
  - ![image](https://user-images.githubusercontent.com/41967014/163308963-43282103-51a1-4bf8-908b-cb1858f5750c.png)
  - ![image](https://user-images.githubusercontent.com/41967014/163308981-d2216c58-7ac7-40f3-a187-5eadbaea7466.png)
  - ![image](https://user-images.githubusercontent.com/41967014/163309139-23a0f0cf-59fc-4e8d-97a6-c7400aaef872.png)

### Reference
- https://arxiv.org/abs/2203.02155
- https://openai.com/blog/instruction-following/
- https://jiho-ml.com/weekly-nlp-53/
