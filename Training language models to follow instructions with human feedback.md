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
  - Our models generalize to the preferences of “held-out” labelers that did not produce any training data.
  - Public NLP datasets are not reflective of how our language models are used.
  - InstructGPT models show promising generalization to instructions outside of the RLHF fine- tuning distribution.
  - InstructGPT still makes simple mistakes.

### 3. Method and experimental details
- InsturctGPT를 훈련시키기 위한 핵심 기술은 reinforcement learning from human feedback (RHHF)
![image](https://user-images.githubusercontent.com/41967014/163312061-46440ccb-8a37-4409-80f7-d0fc6aa3376e.png)
- **Method**
  - Step1 : supervised fine-tuning (SFT)
  - Step2 : reward model (RM) training
  - Step3 : reinforcement learning via proximal policy optimization (PPO)
- **Models**
  - Supervised fine-tuning (SFT)
  - Reward modeling (RM)
  - Reinforcement learning (RL)
- **Workflow**
   - 먼저, GPT-3를 fine-tuning 시킵니다. 이 때 40명의 작업자를 고용하여 레이블 데이터를 생성합니다.
   - 실제 OpenAI API의 유저들이 제출한 프롬포트 중에서 개인정보가 담겨져 있지 않은 예시들을 추출하여 작업자들에게 전달합니다.
   - 10,000여쌍의 데이터를 모아서 GPT-3를 fine-tuning 진행하였습니다.
   - 학습된 GPT-3가 생성한 답변 후보 여러개에 대하여 작업자들이 점수를 매기는 작업을 합니다. 여러 답변 후보들 중에 어떤 답변이 더 좋은 답변인지 순위를 매깁니다.
   - 이러한 작업의 이유는 GPT-3에게 어떤 답변이 더 좋은 답변인지 사람의 피드백을 주기 위함인데 GPT-3의 모든 Output을 사람이 매번 피드백하는 것은 불가능합니다.
   - 따라서 사람의 피드백 점수를 예측하는 모델을 만드는데 이것을 Reward Model이라고 부릅니다.
   - 강화학습에는 다양한 요소들이 존재하는데 Agent(학습하려는 모델), Environment(주변 환경), Action(모델이 취할 수 있는 행동), Policy(모델이 어덯게 행돌할지 결정하는 알고리즘), Reward(모델이 한 행동에 따라 환경에 부여하는 리워드), Interpreter(리워드를 결정하는 사람 또는 시스템) 등이 있습니다.
   - 정리하면, RL은 Agent가 실시간으로 변화하는 Environment에서 Interpreter 의해 결정된 Reward에 의하여 Action을 취하고 그 결과가 Environment에 반영되가면서 최종 Goal을 달성하는 최적의 Policy를 찾아가는 학습 방법입니다.
   - 이를 GPT-3에 대입하면 Agent(GPT-3), Environment(유저의 Input), Action(모델의 Output), Policy(GPT-3의 파라미터), Reward(RM 모델의 예측 점수), Interpreter(RM 모델)이 됩니다.
   - Supervised Learning된 GPT-3 모델이 답변을 생성하면 Reward 모델을 통해 결과를 평가하고 RL 모델의 피드백을 통해 GPT-3의 파라미터를 업데이트 합니다. 이런 과정 계속 반복합니다.
   - 이 때 RL에 사용되는 학습 방법은 Proximal Policy Optimization(PPO)라는 알고리즘인데 Open AI RL 연구팀에서 가장 많이 사용하는 학습 방법입니다.

### 4. Results
- 각각의 모델들의 Output에 대하여 사용자가 1 ~ 7점의 리커트 척도를 이용하여 모델의 결과를 평가. 그 결과 InstructGPT가 가장 높은 선호도를 보임.
![image](https://user-images.githubusercontent.com/41967014/163312160-e7de10ad-0760-4f09-b7b2-c0f03e1ab8be.png)
- truthness, toxic, Hallucinations 등의 안정성 테스트에서도 InstructGPT가 가장 좋은 성능을 보임.
![image](https://user-images.githubusercontent.com/41967014/163312346-3c11e1e5-eb23-4172-9064-d32e6e74cf11.png)

![image](https://user-images.githubusercontent.com/41967014/163308883-72d7f64f-e07f-4629-b107-e30e50ed1d1a.png)
![image](https://user-images.githubusercontent.com/41967014/163308908-3cc06253-645e-4314-bccc-2e7b2ebf0f33.png)
![image](https://user-images.githubusercontent.com/41967014/163308918-a1438b68-d19b-41c1-bb8a-bff4f9f11a48.png)
![image](https://user-images.githubusercontent.com/41967014/163308963-43282103-51a1-4bf8-908b-cb1858f5750c.png)
![image](https://user-images.githubusercontent.com/41967014/163308981-d2216c58-7ac7-40f3-a187-5eadbaea7466.png)
![image](https://user-images.githubusercontent.com/41967014/163309139-23a0f0cf-59fc-4e8d-97a6-c7400aaef872.png)

### Reference
- https://arxiv.org/abs/2203.02155
- https://openai.com/blog/instruction-following/
- https://jiho-ml.com/weekly-nlp-53/
- https://littlefoxdiary.tistory.com/101?category=847374
