# GAN (Generative Adeversarial Network)



#### 개요

- 생성자와 판별자를 경쟁적으로 학습시켜 새로운 데이터를 생성하는 비지도학습 기반 생성모형
- 이미지 생성, 이미지 인페이팅, 추상화 추론, 의미론적 분할, 비디오 생성, 텍스트로부터 이미지 생성 등에 활용



참가자 2명의 제로섬 게임에 기초를 둔다.

- 생성자(G), 판별자 (D)로 구성
- 생성자는 진짜 데이터와 생성자에 의해 만들어지는 가짜데이터를 구별할 수 없도록 판별자를 속이려 한다.
- 판별자는 진짜 데이터를 가짜 데이와 구별하기 위해 학습한다.
- 최적의 해결책은 **내시 균형**에 의해 주어진다
  - 내시 균형 : 참가자들이 최선의 선택을 하면 서로가 자신의 선택을 바꾸지 않는 균형 상태



### GAN 의 이익함수

- 최대극소화 전략 : 2인 영합게임에서, 한 참가자가 입을 수 있는 최대 손실을 최소화하는 전략

- 만약 참가자의 이익과 손실의 합이 0이 되는 영합 게임이라면, 최대극소화 전략 == 최소극대화 전략

- 이익함수(or 손실함수)

  - 판별자가 진짜 데이터와 가짜 데이터를 정확하게 분류하도록 학습시켜야 함

  - 동시에 log(1-D(G(x)))를 최소화하도록 생성자를 학습

  - 최종 이익함수

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221110151018571.png" alt="image-20221110151018571" style="zoom:80%;" />

    - x: 확률분포 P<sub>x</sub>(x)로부터 관측된 진짜 데이터  //  z : 사전분포 P<sub>z</sub>(z)로부터 생성된 잡음

### 학습

- 판별자가 진짜 데이터 x에 대해 1을 출력하고, 잡음표본 z를 기반으로 생성자가 만든 가짜에 대해서는 0 출력
  - D(x)를 가능한 1에 가깝도록 => D(x) 최대화
  - D(G(z))는 0에 가깝도록 => log(1-D(G(z))) 최대화
  - 판별자는 x와 y의 전체 분포에 대해 이를 수행하므로 기댓값을 사용
- 생성자는 판별자의 이익함수 U(D,G)의 두번째 항에 **G(z)** 형태로 나타남
  - 생성자는 판별자의 이익을 최소화하는 전략을 선택 => 생성자의 이익함수 V(D,G) = -U(D,G)
    - 생성자는 U(D,G)를 최소화하는 전략 선택



#### 알고리즘

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221110152112396.png" alt="image-20221110152112396" style="zoom:80%;" />

- k : 하이퍼 파라미터



##### 기본 GAN의 문제점

- 비수렴성 : 내시 군형 문제에서는 우수한 성능 But, 경사하강법은 볼록 문제에서만 내시군형 유지
  - 즉, 완벽한 균형을 확보하기 어려워서 **매개변수가 진동, 불안정**
- 모드 붕괴 : 최소-최대 게임이므로 분명한 손실함수가 없다.
  - 즉, 생성자가 개선되는지 판단하기 어려움 => 같은 데이터를 생성하여 극솟값에 빠지도록 만들어지는 문제
- 기울기 소멸 문제 : 판별자가 너무 대단해서 생성자의 기울기가 사라져버리면서 학습이 안되는 문제
- 하이퍼 파라미터에 매우 민감







## 기본 경쟁적 생성망의 분석

**정보량(divergence)** : 2개의 분포 P, Q의 차이를 나타내는 대표적인 측도



GAN 관련 divergence

![image-20221110152713981](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221110152713981.png)



### LSGAN (Least Square GAN)

- 기본 GAN 판별자는 sigmoid, CrossEntropy 사용

  - 기울기 소멸 문제 발생

    ![image-20221110155625035](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221110155625035.png)

    - 결정 경계 기준으로 오른쪽은 기울기가 거의 0에 가까워서 업데이트가 되기 힘듬

    - LSGAN 등장

- LSGAN

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221110155750465.png" alt="image-20221110155750465" style="zoom:67%;" />

  - a,b :  가짜데이터, 진짜데이터의 라벨.
  - c : 생서앚가 가까데이터에 대해 판별자가 믿기를 바라는 값
    - ex . a,b,c = 0,1,1 => 판별자는 진짜데이터를 1, 가짜데이터를 0으로 분류하고, 생성자는 판별자가 1로 분류하는 가짜데이터를 생성

  ![image-20221110160048769](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221110160048769.png)

##### 문제점

- 모드 붕괴 문제는 여전히 존재
- 복잡한 이미지 문제에서는 제대로 생성을 못함 => 판별자 또한 저품질의 데이터만 받았기 때문에 판별역할을 잘 못함

### DCGAN (Deep Convolution GAN)

- GAN에 CNN을 적용한 기법. 고화질 이미지를 생성할 수 있는 모형.

- 5가지 가이드라인

  - 판별자의 풀링층을 **보폭합성곱**, 생성자의 풀링층을 **전치 합성곱**으로 대체
    - 보폭 합성곱 : 보폭이 2보다 큰 합성곱
    - 전치 학성곱(분수 보폭 합성곱) : 분수 보폭을 사용하는 합성곱
  - 판별자와 생성자에 배치정규화 적용
  - 심층구조를 위해 완전연결 은닉층 제거
  - 생성자는 활성함수를 출력층만 tanh, 나머진 전부  ReLU함수 사용.
  - 판별자의 경우 모든 층에 활성함수로 leaky-ReLU사용

- 구조

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221115150518115.png" alt="image-20221115150518115" style="zoom:80%;" />

  - 판별자는 컨볼루션 연산을 통해 이미지를 축소, 생성자는 분수 컨볼루션 연산을 통해 이미지를 확대

  - 판별자는 진짜 이미지면 1, 가짜면 0을 반환

  - 딥러닝에서의 풀링층은

    - 매개변수를 줄이고
    - 중요한 특징만 골라내는 역할

    을 하는 층이지만, 이미지의 위치 정보를 잃어버리는 단점을 가짐.

    - 따라서 이미지 생성을 위해선 위치 정보가 중요하므로 풀링층을 배제.

  - 미니배치 정규화 사용

    - 미니배치 정규화 : 미니배치 단위로 정규화 => 학습속도 가속화

- 잠재 공간 이용

  - 검증 방법 중 하나. 잠재 공간에 진짜 데이터의 특성이 투영되었는지 살펴보는 것

  - 생성자의 입력인 잡음벡터 z의 값을 바꿔보는 것으로 추력이미지의 속성을 바꿀 수 있다.

  - DCGAN으로 학습된 생성자는 벡터 산술 연산이 가능, 의미론적 수준의 이미지 생성 가능.

  - 가짜 이미지 G(z)와 잡음z의 관계

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221115151220958.png" alt="image-20221115151220958" style="zoom:80%;" />

    - 잠재 공간에서 얼굴 방향에 해당하는 특성을 찾아내서, 벡터 z에서 이에 해당하는 값을 바꿈으로써 이미에서 얼굴이 바라보고 있는 방향을 바꿀 수 있다는 것을 보여줌

- 단, 제대로 된 그림이 아닌 노이즈가 나타날 경우가 많음

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221115153756946.png" alt="image-20221115153756946" style="zoom:80%;" />

- 판별자와 생성자의 손실함수 차가 큼 (불균형 발생)

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221115153825709.png" alt="image-20221115153825709" style="zoom:80%;" />



### WGAN (와서스타인 GAN)

- 와서스타인 정보량을 목적함수로 이용
  - 그러기 위해 클리핑을 사용



#### 와서스타인 정보량

- 두 개의 분포를 일치시키기 위해 필요한 최소 운동량.

- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221115151754076.png" alt="image-20221115151754076" style="zoom:67%;" />

  - P<sub>x</sub>(1)의 값 0.3에서 0.1만큼 P<sub>y</sub>(2)로 이동.

  - P<sub>x</sub>(3)의 값 0.6에서 0.2만큼 P<sub>x</sub>(2)로 이동.

    - 이 때 와서스타인 정보량 : **D<sub>w</sub>(P<sub>x</sub>||P<sub>y</sub>)** = 0.1 x 1 + 0.2 x 1 = **0.3**

      ![image-20221115152625166](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221115152625166.png)

- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221115152906713.png" alt="image-20221115152906713" style="zoom:67%;" />

  - 저차원 다양체의 지지집합을 가진 확률분포를 학습할 때,
    - KL, JS, 총변동 정보량은 합리적인 목적함수가 될 수 없지만 와서스타인 정보량은 합리적인 목적함수가 될 수 있음



<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221115154853849.png" alt="image-20221115154853849" style="zoom:67%;" />

- 알고리즘

  ![image-20221115154925627](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221115154925627.png)

- 기울기 소멸 문제 해결

  ![image-20221115155022753](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221115155022753.png)

- 불균형이 어느정도 해소된 손실함수 차이

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221115155154631.png" alt="image-20221115155154631" style="zoom:67%;" />



### CGAN ()

- WGAN까지 와서 진짜 이미지와 유사한 가짜 이미지를 생성하는 능력을 보여줌.
  - 하지만 사용자가 원하는 특징을 지닌 이미지를 생성하도록 조정할 수 없음
    - ex. 개, 고양이, 말 등으로 훈련하면 고양이만 생성하도록 할 수 없음
  - 이런 문제를 해결하기 위해  등장

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221117151728977.png" alt="image-20221117151728977" style="zoom:80%;" />

- 판별자에 제공되는 조건은 가짜데이터와 진짜 데이터에서 사용되는 용도가 조금 다름
  - 가짜 데이터에 제공된 조건은 생성자에서처럼 단지 가짜데이와 결합될 뿐
  - 진짜 데이터에서는 y에 해당하는 x를 랜덤 추출하여 이와 결합
- CGAN의 최적화 문제
  - ![image-20221117152145051](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221117152145051.png)
- 픽스투픽스, styleGAN 과 같은 스타일 변환 방법의 기초가 됨
  - 이미지를 다른 스타일의 이미지로 변환하기
  - 텍스트로 이미지를 생성하기
  - 비디오 생성 및 예측



### infoGAN (informantion GAN)

#### 상호정보량

- 두 개의 연속형 확룰 벡터 X,Y의 상호정보량의 정의

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221117152900040.png" alt="image-20221117152900040" style="zoom:80%;" />

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221117153056135.png" alt="image-20221117153056135" style="zoom: 67%;" />



#### infoGAN

- 상호정보량을 이용하여 기본 GAN을 확장한 것

- 기본 개념은 **생성자의 입력 벡터를 수정**하는 것

- **생성자는 잡음 벡터 z 와 잠재코드 c의 결합에 기초하여 가짜데이터를 생성**한다. 따라서 생성자는 G(z)가 아닌 G(z,c)가 된다.

  - infoGAN은 잠재코드 c가 데이터에 존재하는 특징을 가질 수 있게 학습

    ![image-20221117153601213](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221117153601213.png)

- ![image-20221117153749545](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221117153749545.png)

