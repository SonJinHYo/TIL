# 딥러닝 최적화 개념정리

- 기계학습에서의 최적화
  - 훈련집합으로 학습을 마친 후, 현장에서 발생하는 새로운 샘플을 잘 예측해야함. (일반화 능력이 뛰어나야 함)
  - 훈련집합이 테스트집합의 대리자 역할을 한다고 말할 수 있음
  - MSE, 로그우도와 같은 목적함수가, 궁극 목표인 정확률의 대리자 역할을 함
- 기계학습의 최적화가 어려운 이유
  - 목적함수의 비볼록 성질,  고차원 특징 공간, 데이터의 희소성
- 이 문서는 이러한 어려움을 극복하는 여러가지 방안을 제시



## 목적함수 : 교차 엔트로피와 로그우도

### MSE 돌아보기

- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011141132386.png" alt="image-20221011141132386" style="zoom:50%;" />

- 오차가 클 수록 e값이 크므로 벌점으로는 좋은 함수

- 단점 : 업데이트 방식이 **gradient를 더해나가는것**

  - 오차가 크더라도 gradeint가 작다면 변화율이 작아져버림

  - 비효율적인 학습률을 유도
  - 대체안으로 교차 엔트로피가 등장



### 교차 엔트로피

- 레이블에 해당하는 y가 확률변수 (부류가 2개 라면, y = 0 or 1)
- 확률 분포 : P는 정답 레이블 , Q는 신경망 출력
  - P(0) = 1-y , Q(0) = 1-o
  - P(1) = y, Q(1) = o
- 교차 엔트로피 식
  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011141820433.png" alt="image-20221011141820433" style="zoom:80%;" />

- 교차 엔트로피 목적함수
  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011141917501.png" alt="image-20221011141917501" style="zoom:80%;" />
- MSE와 다르게 더 큰 오차에 대해 더 큰 벌점을 부여한다.





### softmax 활성함수

- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011142344968.png" alt="image-20221011142344968" style="zoom:80%;" />

- 최댓값은 활성화하고 작은 값은 억제
- 모두 더하면 1이 되므로 확률을 모방
- 로지스틱 시그모이드 : 결과가 0 or 1
- softmax : 결과가 0~1의 실수





### 로그우도 목적함수

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011142531511.png" alt="image-20221011142531511" style="zoom:80%;" />

- 모든 출력 노드값을 사용하는 MSE, 교차엔트로피와 달리 o<sub>y</sub>라는 하나의 노드만 사용



- softmax는 최댓값이 아닌 값을 억제하고, 학습 샘플이 알려주는 부분만 학습한다는 점에서 둘이 잘 어울린다.





## 성능 향상을 위한 요령



### 1 . 데이터 전처리



#### 상황1. 규모문제와 양수 문제

- 규모 문제
  - 키 단위가 m라면, 키차이는 소수단위, 몸무게가 kg단위라면 두자릿수 단위로 차이가 남
    - 이런 경우는 규모가 100배 차이가 나게 됨 -> 학습속도도 100배 차이남

- 모든 특징이 양수인 경우


  - 가중치를 업데이트 할 때 모든 가중치가 같이 증가, 같이 감소

###### 해결법

 - 정규화 (표준화) 해주기

      

#### 상황2. 특징벡터가 많을 때

-  특징벡터 안에 성별/ 체질(태양인,태음인,소양인,소음인)이 있을 때'

###### 해결법

- 명칭값을 원 핫 코드로 변환
  - 명칭값은 거리 개념이 없음
  - 원 핫 코드는 값의 개수만큼 비트를 부여
    - (1,0,0,0,1,0) : (1,0) = 남성 / 0,0,1,0 = 소음인 같은 방법

- 성별,체질 같은 분류가 된 데이터는 비트로 바꿔주는것





### 2.가중치 초기화

- 대칭적 가중치 문제
  - 대칭적 가중치 : 가중치가 대칭구조로 나타나는것. 두 노드가 같은 일을 해버리는 상황 발생
  - 가중치를 난수로 초기화하여 대칭성을 파괴. 가우시안분포나 균일 분포에서 난수 추출
    - -r ~ r 까지에서 난수 생성
    - r = n<sub>ij</sub><sup>-1/2</sup>
    - bias는 보통 0으로 초기화

- AlexNet, ResNet에서 사용





### 모멘텀

- gradient의 잡음 현상
  - gradient를 추정하므로 잡음 가능성이 높다
  - 모멘텀은 그레디언트에 스무딩을 가하여 잡음 효과 줄임 -> 수렴 속도 향상
- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011143951723.png" alt="image-20221011143951723" style="zoom:67%;" />
  - v : 이전 그레디언트의 누적값 (시작값 = 0)
  - 알파의 효과
    - 0일 때, 모멘텀이 적용 안된 이전 공식과 같음
    - 1에 가까울수록 이전 그레디언트가 큰 효과를 보이게 됨
  - 네스테러프 모멘텀
    - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011144124801.png" alt="image-20221011144124801" style="zoom:80%;" />
    - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011144323223.png" alt="image-20221011144323223" style="zoom:80%;" />



### 적응적 학습률

- **학습률의 중요성**
  - 너무 크면 오버슈팅에 따른 진자 현상, 너무 작으면 수렴이 느림
- AdaGrad
  - 오래된 그레이디언트와 최근 그레디언트가 같은 비중의 역할
    - r이 점점 커져 수렴 방해할 가능성이 있음
    - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011144833597.png" alt="image-20221011144833597" style="zoom: 67%;" />
- RMSProp 
  - 가중 이동 평균 기법 적용
  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011144806483.png" alt="image-20221011144806483" style="zoom:50%;" />
- Adam
  - RMSProp에 모멘텀을 추가
  - 

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011144941036.png" alt="image-20221011144941036" style="zoom:67%;" />



### 활성 함수

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011145028755.png" alt="image-20221011145028755" style="zoom:67%;" />

- ReLU 활성함수
  - 포화 문제를 해소
  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011145209541.png" alt="image-20221011145209541" style="zoom:67%;" />







## 규제의 필요성과 원리



### 과잉적합에 빠지는 이유

- 대부분의 경우, 가지고 있는 데이터에 비해 훨씬 큰 용량의 모델을 사용
  - VGC만 해도 매개변수가 1.2억개
  - 훈련집합은 단순히 '암기'하는 과잉적합에 주의를 기울여야 함
- 현대 기계의 학습 전략
  - 충분히 큰 용량의 모델을 설계한 다음, 학습 과정에서 여러 규제 기법을 적용



### 규제의 정의

- 수학, 통계학에서의 규제
  - 모델 용량에 비해 데이터가 부족한 경우 불량 문제를 푸는 데 사용
  - 적절한 가정을 투입하여 문제를 품 -> 입출력 사이의 매핑은 매끄럽다는 사전 지식
  - 티호노프의 규제 기법은 매끄러움 가정에 기반을 둔 식을 사용
    - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011173210580.png" alt="image-20221011173210580" style="zoom:50%;" />



### 규제 기법

#### 1. 가중치 벌칙

- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011173351321.png" alt="image-20221011173351321" style="zoom:50%;" />

  - 규제항은 훈련집합과 무관, 데이터 생성 과정에 내재한 사전 지식에 해당한다.

  - 규제항은 매개변수를 작은 값으로 유지하므로 모데 용량을 제한하는 역할

- 규제항 R 주로 L2, L1 norm을 사용한다.

  -  큰 가중치에 벌칙을 가해 작은 가중치를 유지하기 위해

- L2 norm

  - **가중치 감쇠** : 규제 항 R로 L2놈을 사용하는 규제 기법

  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011173605181.png" alt="image-20221011173605181" style="zoom:50%;" />

  - 그레디언트 계산

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011173633499.png" alt="image-20221011173633499" style="zoom:50%;" />

- L1 norm

- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011173750979.png" alt="image-20221011173750979" style="zoom:67%;" />





### 조기 멈춤

- 학습 시간에 따른 일반화 능력

  - 일정 시간이 지나면 과잉적합 현상이 나타남 -> 일반화 능력 저하
  - 훈련 데이터를 암기하기 시작한다고 볼 수 있음

- 조기 멈춤 : 검증집합의 오류가 최저인 점 t<sub>opt</sub>에서 학습을 멈춤

- Before

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011173956904.png" alt="image-20221011173956904" style="zoom:80%;" />1

- After

  

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011174036222.png" alt="image-20221011174036222" style="zoom:80%;" />





### 데이터 확대

- 과잉적합을 방지하는 가장 확실한 방법은 큰 훈련집합을 사용하는 것

  - 하지만 데이터 수집 비용이 많이 들어감

- 데이터 확대 규제기법

  - 데이터를 인위적으로 변형하여 확대
  - 자연계에서 벌어지는 잠재적인 병형을 프로그램으로 흉내 내는 

  1. MNIST의 affine 변환

     - translation, rotaion, scaling 을 적용

     - 한계점
       - 수작업 변형
       - 모든 부류가 같은 변형을 사용

  2. morphing(모핑) 변형
     - 비선형 변환으로서 어파인 변환에 비해 훨씬 다양함
     - 데이터에 맞는 **'비선형 변환 규칙을 학습'**
  3. 자연영상 확대
     - ex. 256 x 256 영상에서 224 x 224 을 1024장 잘라냄 => 이동효과
     - 그 상태에서 좌우 반전 시도 => 2048배의 데이터 확대 효과
  4. 잡음을 섞어 확대하는 기법
     - 입력데이터에 잡음을 섞는 기법
     - 은닉 노드에 잡을을 섞는 기법 ( 고급 특징 수준에서 데이터를 확대하는 셈)





### 드롭 아웃

- 드롭아웃 규제 기법

  - 입력층과 은닉층의 노드 중 일정 비율을 임의로 제거

  - 남은 **부분 신경망**을 학습

  - 많은 **부분 신경망**을 만들고, 예측 단계에서 앙상블 결합하는 기법

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221013170718319.png" alt="image-20221013170718319" style="zoom:67%;" />

  - 많은 부분신경망을 학습, 저장, 앙상블 결합하는데 따른 시간적, 공간적 비용부담이 크다







### 앙상블 기법

- 앙상블
  - 서로 다른 여러 개의 모델을 결합하여 일반화 오류를 줄이는 기법
  - 현대 기계학습은 앙상블도 규제로 여긴다

- 두 가지 일
  - 서로 다른 예측기를 학습하는 일 
    1. ex. 서로 다른 구조의 신경망 여러 개를 학습 또는 같은 구조를 사용하되, 서로 다른 초긱값과 하이퍼파라미터를 설정후 학습
    2. ex. 배깅 : 훈련집합을 여러 번 샘플링하여 서로 다른 훈련집합을 구성
    3. ex. 부스팅 (i번쨰 예측기가 틀린 샘플을 i+1번째 예측기가 잘 인식하도록 연계성을 고려
  - 학습된 예측기를 결합하는 일
    - 주로 투표 방식을 사용







## 하이퍼 매개변수 최적화



#### 학습 모델이 가지는 파라미터 2 종류

1. 내부 매개변수
   - 신경망의 경우 에지 가중치로서 대문자 theta로 표기
   - 학습 알고리즘이 최적화함
2. 하이퍼 매개변수
   - 모델의 외부에서 모델의 동작을 조정함
   - ex. 은닉층 개수, CNN 마스크 크기,보폭 , 학습률, 모멘텀관련 파라미터



#### 하이퍼 파라미터 선택 고려사항

1. 보통은 표준 문서에서 제시하는 기본값을 사용
2. 후보 값 중에서 주어진 데이터에 최적인 값 선택
   - 하이퍼 매개변수 조합을 생성
     1. 수동 탐색, 격자 탐색, 임의 탐색 등등 의 방법을 통해 생성
3. 로그 규모 간격
   - 어떤 매개변수는 로그 규모를 사용해야 함
     - 0.00001~1까지 검사할 때, 하나씩이 아닌 값을 2배씩 증가시키는 방식을 써줘야 함
4. 차원의 저주
   - 임의 탐색 사용
