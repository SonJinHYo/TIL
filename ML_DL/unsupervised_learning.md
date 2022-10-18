# 비지도 학습

지도학습 : 훈렵집합 X,Y가 주어짐

비지도 학습 : 훈련집합 X만 주어지

- 사용 : 맞춤 광고, 차원 축소, 데이터 압축, 데이터 가시화, 특징 추출, 생성 모델 구축



## 지도 학습, 비지도 학습, 준지도 학습



**학습 유형**

- 지도 학습 : 모든 훈련 샘플이 레이블 정보를 가짐

- 비지도 학습 : 모든 훈련 샘플이 레이블 정보를 가지지 않음

- 준지도 학습 : 레이블을 가진 샘플과 가지지 않은 샘플이 섞여 있음

  

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018151532869.png" alt="image-20221018151532869" style="zoom:67%;" />

**기계학습이 사용하는 두 종류의 지식**

- 훈련집합
- 사전 지식(세상의 일반적인 규칙). 아래는 두 가지 중요 사전 지식
  - 매니폴드 가정 : 데이터집합은 하나의 매니폴드 or 여러 개의 매니폴드를 구성하며, 모든 샘플은 매니폴드와 가까운 곳에 있다.
  - 매끄러움 가정 : 샘플은 어떤 요인에 의해 변화한다
    - 변화는 갑자기 확 튀는 것이 아닌 과정을 통해 매끄럽게 변화한다는 가정

- 비지도, 준지도 학습은 사전 지식을 더 명시적으로 사용한다.



## 비지도 학습의 일반 과업


### 1. 군집화 : 유사한 샘플을 모아 같은 그룹으로 묶는 일 

- 주어진 데이터 X집합을 군집집합 C를 찾아내는 작업

  - 군집 개수 k는 주어지는 경우와 자동으로 찾아야 하는 경우가 있음
  - 부류 발견 작업이라고 부르기도 한다

- 맞춤 광고, 영상 분할, 유전자 데이터 분석, SNS 실시간 검색어 분석

- 군집화는 주관성을 가짐

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018152453714.png" alt="image-20221018152453714" style="zoom:50%;" />

#### k-평균 알고리즘

- 원리가 단순하지만 성능이 좋은 편 / 직관적으로 이해하기 쉽고 구현이 쉬움 / 군집 갯수 k를 알려줘야 함

- 알고리즘

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018152606143.png" alt="image-20221018152606143" style="zoom:67%;" />

  - 요약
    1. 군집 중심 k개를 초기화를 한다.
    2. 데이터를 각 군집 중심에 가까운 곧으로 배정
    3. 배정된 군집의 평균을 다시 군집 중심으로 한다
    4. 군집 중심이 이전과 같다면 탈출

- **k-medoids** : 군집의 평균이 아닌 대표를 뽑아서 진행

  - 유난히 튀는 잡음데이터에 둔감함

- 최적화 문제로 해석

  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018153224337.png" alt="image-20221018153224337" style="zoom:67%;" />



#### **다중 시작 k-평균**

- 차이점 : **서로 다른 초기 군집 중심을 잡고 여러번 수행**을 한 다음, 가장 좋은 품질의 해를 취함

  - k-평균 알고리즘은 초기값에 의해 결과가 달라지기 때문

- 알고리즘

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018153820671.png" alt="image-20221018153820671" style="zoom:67%;" />

#### PCA기반 얼굴 인식

- 관측치로부터 분산 값이 큰 축을 추출하는 과정 (분산이 큰 축을 주축으로 정의)

  - 보통 2개의 주축을 잡는다면 직교하게 잡는다. (기준 벡터가 직교하게 잡는다)

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018154028025.png" alt="image-20221018154028025" style="zoom:50%;" />

- 나머지 데이터를 주축으로 투영시키는 것이 PCA기반 얼굴 인식의 대략적인 차원 축소

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018154311272.png" alt="image-20221018154311272" style="zoom:67%;" />

- 어떤 벡터 x 하나가 기준 벡터 e<sub>j</sub>에 투영되었을 때 크기

  - <img src="../../AppData/Roaming/Typora/typora-user-images/image-20221018154401015.png" alt="image-20221018154401015" style="zoom:50%;" />
    - x라는 벡터에 e<sub>j</sub> 성분이 a<sub>j</sub>만큼 존재

- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018154540136.png" alt="image-20221018154540136" style="zoom:67%;" />

- 다양한 특징 벡터 축 중 데이터를 가장 잘 표현할 수 있는 축 인식에 사용

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018154646199.png" alt="image-20221018154646199" style="zoom: 80%;" />



- 학습 영상

  - P개의 2차원 얼굴 영상, m x n 개의 화소롤 구성 (N = m x n)

  - 얼굴 인식 과정

    1. 학습 영상에서 평균 얼굴 제거
       - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018155241141.png" alt="image-20221018155241141" style="zoom:67%;" />
    2. 공분산 행렬의 고유벡터 계산
       - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018155301626.png" alt="image-20221018155301626" style="zoom:67%;" />
    3. 고윳값이 큰 고유 벡터에 투영
       - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018155319875.png" alt="image-20221018155319875" style="zoom:67%;" />
    4. 1의 결과를 고유 벡터에 투영
       - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018155337481.png" alt="image-20221018155337481" style="zoom:67%;" />
    5. 결과값의 유사도를 통한 인식

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018155005549.png" alt="image-20221018155005549" style="zoom:67%;" />





#### 2. 밀도 추정 : 데이터로부터 확률분포를 추정하는 일

   - 분류, 생성 모델 구축

#### 3. 공간 변환 : 원래 특징 공간을 저차원 or 고차원 공간으로 변환하는 일

   - 데이터 가시화, 데이터 압축, 특징 추출 등

**데이터에 내재한 구조를 잘 파악하여 새로운 정보를 잘 파악해내야 함**

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018152111712.png" alt="image-20221018152111712" style="zoom:67%;" />
