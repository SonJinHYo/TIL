# RNN

- 시간성 데이터
  - 특징이 순서를 가지므로 **순차 데이터**라고도 부름
  - 순차데이터는 동적, 가변 길이를 가짐
- 주식 차트, 문장, 음성 신호 등등

- 순환 신경망
  - 시간성 정보를 활용하여 순차 데이터를 처리하는 효과적인 학습 모델
  - 매우 긴 순차 데이터를 처리하는 데에는 장기 의존성을 잘 다루는 LSTM을 주로 사용





## 순차데이터 표현

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221117151019340.png" alt="image-20221117151019340" style="zoom:50%;" />

- 일반적인 표기 : 벡터의 벡터 (벡터를 요소로 가지는 벡터)

#### 텍스트 순차데이터

- April is the cruelest month --> 사월은 가장 잔인한 달, 로 번역하는 방법은?
  - 사전 구축 방법
    - 사람이 사용하는 단어를 모아 구축 or 말뭉치를 분석하여 단어를 자동 추출하여 구축
  - 사전을 사용한 텍스트 순차데이터 표현 방법
    - 단어가방 : 단어 각각의 빈도수를 세어 m차원의 벡터로 표현 (m : 사전 크기)
      - 한계 : 시간성 정보 휘발
    - 원 핫 코드 : 해당 단어의 위치만 1로 표시
      - 한계1 : 한 단어를 표현하는데 m차원 벡터 사용 (비효율성)
      - 한계2 : 간 유사도 측정 불가능
    - 단어 임베딩 : 단어 사이의 상호작용을 분석하여 새로운 공간으로 변환 (m보다 훨씬 낮게). 변환 과정은 말뭉치를 훈련집합으로 사용하여 알아냄



### 표준신경망, 순환신경망

- 표준 신경망은 먼 과거데이터는 기억하지 못함
  - 새로운 구조의 필요 => RNN



- 순환신경망 기능
  - 가변 길이의 입력을 처리할 수 있어야 함.
  - 장기 의존성을 추적할 수 있어야 함.
  - 순서에 대한 정보를 유지.
  - 시퀀스 전체에 파라미터를 공유할 수 있어야 함.
- 순환 데이터를 일정한 길이로 잘라서 여러개의 훈련 샘플을 만들어야 함



#### RNN 구조와 동작

- **feed-forward neural network**  : 기존 신경망

-  **RNN** : 순환신경망

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221129150935717.png" alt="image-20221129150935717" style="zoom:67%;" />

- 다음 단어 예측 예시

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221129151600302.png" alt="image-20221129151600302" style="zoom:67%;" />

- RNN 구조에서는 **은닉층**이라는 용어 대신 **은닉 상태**, **입출력층** 대신 **입출력 벡터**라고 표현

- 활성 함수 : tanh

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221129151738965.png" alt="image-20221129151738965" style="zoom:67%;" />



#### RNN 유형

- **One to One**
  - 단일 입력, 단일 출력의 구조. 가장 일반적인 신경망으로 Vanila Neural network 이라 부른다.
  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221129151905144.png" alt="image-20221129151905144" style="zoom:50%;" />

- **One to Many**
  - 하나의 입력을 받아서 많은 수의 출력.
  - 하나의 이미지가 입력되면 그것을 잘 설명하는 캡션들이 생성
  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221129151959050.png" alt="image-20221129151959050" style="zoom:50%;" />
- **Many to One**
  - 일련의 입력을 받아 단일 출력을 생성.
  - 대표적으로 감정 분석 신경망 타입이 있음. (문장의 부정 or 긍정 분류)
  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221129152141377.png" alt="image-20221129152141377" style="zoom:50%;" />
- **Many to Many**
  - 많은 수의 입력을 받아 많은 수를 출력.
  - 기계 번역이 이에 속함 (단어들을 다른 단어들로 계속 출력)
  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221129152248120.png" alt="image-20221129152248120" style="zoom:50%;" />



#### RNN 순전파,역전파

- 순전파, 역전파 (Mant to One)

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221129153328702.png" alt="image-20221129153328702" style="zoom:80%;" />

  - keras code 

    ```python
    inputs = np.random.random([32,10,8]).astype(np.float32)
    
    simple_rnn = tf.keras.SimpleRNN(4) # hidden_size
    output = simple_rnn(inputs)
    ```

    - SimpleRNN의 입력 : [batch, timesteps, feature]

- 역전파(Backpropagation Through Time : BPTT)

  - 문제점 : 1보다 작은 값이 여러번 곱해지면 그레이디언트 소실, 1보다 크면 그레이디언트 폭증이 일어남
    - 그 결과, 근거리 의존 관계만을 중시하게 됨.
  - 그레이디언트 소실 방안
    - 활성화 함수를 ReLU로 바꾸면 도움이 됨.
    - 가중치를 단위 행렬로 초기화 or 바이어스를 0으로 초기화
    - LSTM, GRU와 같이 Gated Cell을 사용 (어떤 정보를 지나가게 할 것인지 제어 => 장기기억 가능)
  - 그레이디언트 폭증 방안
    - 일정 크기 이상 커지지 못하게 제한





### LSTM(Long short-term memory) 신경망

- 기존 RNN의 그레이디언트 소실 문제를 해결하기 위해 개발

- LSTM 유닛 구성 : 셀, 입력게이트, 출력 게이트, 망각 게이트

- 임의의 시점에 대한 값을 기억하고 **세 개의 게이트는 셀로 들어오고 나가는 정보 흐름을 조절**

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221129154037294.png" alt="image-20221129154037294" style="zoom:80%;" />



#### 게이트

LSTM의 주된 빌딩 블록



##### 저장 연산

- 셀 상태에 관련이 있는 새로운 정보를 저장

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221129154239060.png" alt="image-20221129154239060" style="zoom:80%;" />

##### 삭제 연산

- 예전 상태 중에서 일부 관련이 없는 정보를 지우는 것

- 현재의 입력 **x<sub>t</sub>**와 이전 은닉상태 **h<sub>t-1</sub>**이 sigmoid를 거침.

  - 출력되는 0~1 사이의 값이 셀 상태에서 **삭제를 결정하는 값** (0에 가까울수록 많이 삭제, 1에 가까울수록 유지 비율이 높다)

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221129154538852.png" alt="image-20221129154538852" style="zoom:80%;" />



##### 업데이트 연산

- 선택적으로 셀 상태 값을 업데이트하는 것

- LSTM에서 장기 기억이 저장되는 곳이 셀 상태 **c<sub>t</sub>**이다.

  1. 먼저 셀 상태에 삭제 게이트를 통해 들어오 값을 곱해서 일부 기억을 삭제
  2. 입력 게이트를 통한 값을 더한다. (새로운 기억 추가)

- 즉, 삭제게이트에서 이전 정보를, 입력게이트에서 새로운 정보를 얼마나 기억할지 결정

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221129154836273.png" alt="image-20221129154836273" style="zoom:80%;" />

##### 출력 연산

- 출력 게이트는 **현재 시점의 입력값**과 **이전 시점의 은닉 상태**에 sigmoid 적용

  - 출력값은 다음 시점의 은닉 상태를 결정

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221129155020064.png" alt="image-20221129155020064" style="zoom:80%;" />

