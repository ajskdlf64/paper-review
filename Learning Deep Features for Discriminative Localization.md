# Learning Deep Features for Discriminative Localization

### Class Activation Map (CAM)
![image](https://user-images.githubusercontent.com/41967014/174726406-a3149823-24c6-4933-a073-316a295c9670.png)
- CAM이란 주어진 이미지에 대해서 해당 label로 판별할 수 있는 중요한 feature에 대해서 표시하는 것

**********

### Global Average Pooling (GAP)
![image](https://user-images.githubusercontent.com/41967014/174726582-f0c7552e-6cd7-44df-bd48-45b82beb749c.png)
- 2014년 Network in Network 논문에서 소개
- Classification 과정에서 Fully Connected Layer의 단점은 Label의 정보가 어떤 과정을 거쳐서 역전파되는지 알기 힘듦
- 파라미터가 많아지면 오버피팅의 문제가 발생 -> Dropout 등의 규제 방법에 의존
![image](https://user-images.githubusercontent.com/41967014/174726979-466c6520-5f32-4dff-932a-0674555416e6.png)
- 반면, GAP을 적용하게 된다면,
  - Label과 직관적인 관계 형성
  - 어떤 Feature Map이 높은 Confidence를 갖는지 용이
  - 별도의 파라미터가 필요하지 않음
![image](https://user-images.githubusercontent.com/41967014/174727555-9752dddf-cb8b-4e50-a042-8b2db3d9bca0.png)
- GAP (Global Average Pooling)
  - 이미지의 전체적인 특징을 고려
  - Classification에서 준수한 성능
  - Localization에서 준수한 성능
- GMP (Global Max Pooling)
  - 가장 두드러지는 특징만을 선택
  - Classification에서 준수한 성능
  - Localization에서 낮은 성능

**********

### (Weekly Supervised) Object Localizaton 
![image](https://user-images.githubusercontent.com/41967014/174727986-0ed3f3d5-09c4-4956-b66c-8a4fa34895d0.png)
- Class Label 정보만을 사용하여 객체의 위치를 포착하는 것
- 이미지에 BBox를 치는 것은 high-cost label이고, 단순히 이미지에 label만 부여하는 것은 low-cose label이다.
- low-cost label로 high-cost label을 수행하는 것을 Weekly Supervised Learning이라고 부른다.

|기존 방법론의 문제점|Class Activation Map|
|:------:|:------:|
|Not End-to-End|End-to-End|
|Multiple Forward Pass|Single Forward Pass|
|Global Max Pooling|Global Average Pooling|

**********

### 
