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

### CAM
![image](https://user-images.githubusercontent.com/41967014/174729485-47e8ac57-f64f-4c51-a332-9873f655667b.png)
- Convolutuion 층 바로 다음에 Gloval Averate Pooling(GAP)을 붙이고 Softmax를 연결하는 모델 구조
- 예를 들어 Convolution 층의 마지막 Output이 (width, height, channel) = (256, 256, 128) 인 Activation Map이 있다면, 각 Channel 별로 모두 GAP을 해서 128개의 값이 나오면 softmax로 그 값들을 연결시켜주는 것이다. 이때 각각의 Class로 가는 Weight를 곱해준다.
- 이렇게 하면 128개의 Channel 중에서 해당 Class로 판별하기 위해 어떤 Channel이 중요한 역할을 하는지 판단할 수 있다.
![image](https://user-images.githubusercontent.com/41967014/174729591-0e4b705a-c9f7-4ee3-ac64-c2a39afbf559.png)
- $M_{c}$ = 클래스 c에 대한 Class Activation Map
- 즉, 이미지가 클래스 c로 분류되는 경우, $(x,y)$ 위치에서의 Importance

**********

### Example Outputs
![image](https://user-images.githubusercontent.com/41967014/174729680-badb9db3-ac9b-44dd-8425-b1fb81ba461e.png)

**********

### 서로 다른 Class에 대한 Activation Map
![image](https://user-images.githubusercontent.com/41967014/174730480-03e42edd-b2a8-4116-b290-c09778c63748.png)

