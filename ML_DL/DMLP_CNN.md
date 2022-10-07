# 심층 퍼셉트론 & CNN



## 구조

- 입력층 (d+1)개, 출력층이 c개 

- L-1개의 은닉층 ( 입력층은 0번째 은닉층, 출력층은 L번째 은닉층으로 간주)

  - l번째 은닉층의 노드 수를 n<sub>i</sub> 로 표기

  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004151650204.png" alt="image-20221004151650204" style="zoom:67%;" />

- u<sup>l</sup><sub>ji</sub>  은 l-1번째 층의 i번째 노드와, l번째 층의 j번째 노드를 연결하는 가중치

  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004151936077.png" alt="image-20221004151936077" style="zoom:67%;" />



## 동작 

- MLP의 동작에서 여러번 단계를 확장한 것

  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004152154636.png" alt="image-20221004152154636" style="zoom:50%;" />
  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004152242986.png" alt="image-20221004152242986" style="zoom: 50%;" />
- 학습 과정 또한 MLP에서 단계만 여러번 늘어남



#### 발전 과정 review

1. 퍼셉트론
   - 활성함수 : 계단함수
   - 손실함수 :  평균제곱오차
2. 다층 퍼셉트론(MLP)
   - 활성함수 : 시그모이드 함수
   - 손실함수 : 평균제곱오차
3. 깊은 다층 퍼셉트론(DMLP)
   - 활성함수 : ReLU or 그 변형
   - 교차엔트로피 or 로그우도
4. 이후 CNN이 인식 경쟁에서 DMLP를 앞서나감







# CNN (Convolution Neural Network)

- DMLP

  - 완전연결 구조로 높은 복잡도
  - 학습이 매우 느리고 과잉적합이 일어나기 쉬움

- CNN

  - 컨볼루젼 연산을 이용한 부분연결 구조 => 복잡도를 크게 낮춤
  - 컨볼루젼 연산은 좋은 특징을 추출

## CNN

- 격자 구조(영상, 음성 등)을 갖는 데이터에 적합
- 수용장(receptive field) 은 인간의 시각과 유사
- 가변 크기의 입력 처리가 가능



## 컨볼루션층

- **컨볼루션 연산**

  - 해당하는 요소끼리 곱하고 결과를 모두 더하는 선형 연산

  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004153419064.png" alt="image-20221004153419064" style="zoom:80%;" />
- **덧대기 방식** 

  -  가장자리 입력이 줄어드는 효과 방지

  -  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004153513757.png" alt="image-20221004153513757" style="zoom:67%;" />
- **바이어스 추가** 

  -  상수 더하기

  -  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004153742285.png" alt="image-20221004153742285" style="zoom: 80%;" />
- **가중치 공유**

  - 모든 노드가 동일한 커널을 사용(가중치를 공유)하므로 매개변수는 3개
    - 모델의 복잡도가 크게 낮아짐
  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004154216922.png" alt="image-20221004154216922" style="zoom:50%;" />
- **다중 특징 맵 추출**

  - 커널의 값에 따라 커널이 추출하는 특징이 달라짐
    - 하나의 커널만 사용하면 너무 빈약한 특징이 추출된다.
  - 3개의 커널을 사용하면 3개의 특징 맵이 추출된다.
    - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004154757737.png" alt="image-20221004154757737" style="zoom:80%;" />
    - 커널을 수심~수백개 사용하면 다양한 특징 추출 가능



- **특징 학습**
  - 커널을 사람이 설계하는 것이 아닌 학습으로 알아낸다
  - u<sup>k</sup><sub>i</sub>는 k번째 커널의 i번째 매개변수
  - DMLP와 마찬가지로 오류 역전파로 커널을 학습



- 컨볼류션 연산에 따른 CNN의 특성
  - 이동에 동변 : 신호가 이동하면 이동 정보가 그대로 특징 맵에 반영
    - 영상 인식에서 물체의 이동에 대처 가능
    - 음성 인식에서 발음 지연에 효과적으로 대처
  - 병렬분산 구조
    - 각 노드는 독립적으로 계산 가능하므로 병렬구조
    - 노드는 깊은 층을 거치면서 전체 영향을 미치므로 분산 구조
    - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004155505489.png" alt="image-20221004155505489" style="zoom:67%;" />
      - 층별로 병렬분산은 안되지만, 한 층을 병렬로 구조화 가능



- 큰 보폭에 의한 다운샘플링
  - 지금까지는 모든 화소에 커널을 적용 (빈틈없다는 뜻) == 보폭이 1이다.
  - 보폭이 2라면 한 칸 씩 뛰어넘으면서 특징 추출
    - 보폭이 k라면, 특징 맵이 k<sup>-2</sup>으로 작아진다.
    - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004155746727.png" alt="image-20221004155746727" style="zoom:50%;" />

- 텐서에 적용
  - 3차원 이상의 구조에도 적용 가능함
    - ![image-20221004155851411](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004155851411.png)
  - 사차원 데이터 : RGB데이터 구조의 영상 (t축 추가)
    - ![image-20221004160057966](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221004160057966.png)


## 풀링층

- 풀링 연산을 시행하는 층

- 풀링 연산 : 최대 풀링, 평균 풀링, 가중치 평균 풀링 등이 있음

  - 풀링사이즈가 3이라면,

    - 최대 풀링 : 3개의 값 중 최대를 받는 풀링 (그림(a))

    - 평균 풀링 : 3개의 값의 평균을 받는 풀링

    - 가중치 평균 풀링 : 3개의 값에 가중치를 적용한 다음 평균을 받는 풀링

      <img src="../../AppData/Roaming/Typora/typora-user-images/image-20221006151219307.png" alt="image-20221006151219307" style="zoom:50%;" />

    

  - 풀링은 상세 내용에서 요약통계를 추출한다

  - 매개변수가 없음

  - 특징 맵의 수를 그대로 유지

  - 작은 이동에 둔감. 이는 물체 인식이나 영상 검색 등에 효과적이다

  - 보폭을 크게 하면 다운샘플링 효과를 얻음

## 전체 구조

### 빌딩블록

- CNN은 빌딩블록을 이어 붙여 깊은 구조를 만듬

- 전형적인 빌딩 블록

  - 컨볼루션층 -> 활성함수(주로 ReLU) -> 풀링층

  - 다중 커널을 사용하여 다중 특징 맵을 추출함

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221006150936195.png" alt="image-20221006150936195" style="zoom:67%;" />



### 초창기 CNN - LeNet-5 (1998)

- 특징 추출  : C-P-C-P-C 의 다섯 층을 통해 28*28 명암 맵을 120차원 특징 벡터로 변환
- 은닉층이 하나인 MLP로 분류
- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221006152112673.png" alt="image-20221006152112673" style="zoom:50%;" />
  - s (step) : 보폭
  - h () : ?
  - k(kernel): 커널 갯수
  - p (padding) : 덧대기
- FC층 : 완전연결층. 84개의 은닉층을 가짐



### 가변 크기의 데이터 다루기

- DMLP는 특징 벡터의 크기가 달라지면 연산이 불가능
- CNN은 가변 크기를 다룰 수 있는 강점
  - 컨볼루션층에서 보폭 조정 or 풀링층에서 커널이나 보폭을 조정해서 특징 맵의 크기를 조절 가능





## 컨볼루션 신경망 사례연구



### AlexNet

- 5개의 컨볼루션층, 3개의 완전연결층(FC)

- 컨볼루션층은 200만개, FC층은 6500만개 정도의 매개변수

- FC층에 30배 많은 메개변수를 둠

  - 향후 CNN은 FC층의 매개변수를 줄이는 방향으로 발전

- 성공 요인

  - 외부요인

    - 대용량의 데이터베이스

    - GPU를 사용한 병렬처리

  - 내부요인

    - ReLU사용
    - 지역 반응 정규화 기법 적용
    - 과잉적합을 방지하는 여러 규제 기법 적용
      - 데이터 확대 (잘라내기, 반전으로 2048배 확대)
      - 드롭 아웃 등등





### VGGNet

- 핵심 아이디어

  - 3x3의 작은 커널을 사용하여 신경망을 더욱 깊게 만듬
  - 컨볼루션층을 8~16개를 두어 AlexNet에 비해 2~3배 깊어짐

- 16층 짜리 VGG-16 (컨볼루션층 13개 FC층 3개)

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221006153312671.png" alt="image-20221006153312671" style="zoom:67%;" />

- 1x1 커널

  - 차원 축소 효과

    - 예시)  m x n 특징 맵 8개에 1x1커널 4개 적용하면,  m x n 의 특징맵 4개가 됨

    - 즉, 8 x m x n 텐서에 8x1x1 커널을 4개 적용하여, 4 x m x n텐서를 출력. 길이를 1x1커널로 바꿈

      <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221006153615978.png" alt="image-20221006153615978" style="zoom:50%;" />

  - ReLU 같은 비선형 활성함수를 적용하면 특징 맵의 분별력 증가





### GoogLeNet

- 핵심아이디어 - 인셉션 모듈

  - NIN의 구조를 수정한 것

- NIN 구조

  - MLPconv층이 컨볼루션 연산을 대신함

  - MLPconv는 커널을 옮겨가면서 MLP의 전방 계산을 수행한다

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221006154159817.png" alt="image-20221006154159817" style="zoom:50%;" />

    - 주어진 영역에서만 MLP로 완전연결이 되어있음
      - 이중 구조 -> Network In Network (NIN)

- NIN의 전역 평균 풀링

  - VGC 완전연결층에서 1억천만개 가량의 매개변수를 가지게 됨 -> 과잉적합 원인

  - 전역 평균 풀링
    - MLPconv가 부류 수만큼 특징 맵을 생성하면, 특징 맵 각각을 평균하여 출력 노드에 입력. 이것으로 매개변수를 없앰

- GoogLeNet의 인셉션 모듈

  - 마이크로 네트워크로 MLP -> 네 종류의 컨볼루션 연산 사용

  - 1x1 컨볼루션을 사용하여 차원 축소

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221006154749613.png" alt="image-20221006154749613" style="zoom:50%;" />

  - 인셉션 모듈 9개를 결합

    - 매개변수가 있는 층 22개, 없는 층 5개로 구성

    - 완전 연결 층 1개

    - 백만 개의 매개변수를 가진다. VGG에 비하면 1%

      <img src="../../AppData/Roaming/Typora/typora-user-images/image-20221006154900335.png" alt="image-20221006154900335" style="zoom:67%;" />





### ResNet

- 잔류 학습이라는 아이디어를 이용하여 성능 저하를 피하면서 층 수를 대폭 늘림 (최대 1202층)

- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221006155003443.png" alt="image-20221006155003443" style="zoom:67%;" />

- 지름길을 두는 이유

  - **gradient 소멸 문제 해결**

- 34층짜리 ResNet

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221006155224237.png" alt="image-20221006155224237" style="zoom:80%;" />

- VGGNet 공통점 : 3x3 커널 사용

- VGGNet 차이점 

  - 잔류 학습 사용
  - 전역 평균 풀링 사용 (FC층 제거)
  - 배치정규화 적용 (드롭아웃 적용 불필요)





#### 딥러닝이 강력한 부분

- 고전 방법에서는,

  - 분할, 특징 추출, 분류를 따로 구현한 다음 이어서 붙인다.
  - 사람의 직관에 따르므로 성능 한계, 인식 대상이 달라지면 새로 설계

- 딥러닝은 이 과정을 동시에 최적화

- 깊이의 중요성

  - 실선은 20개 노드를 가진 은닉층 하나짜리 vs 점선은 각각 10개 노드를 가진 은닉층 2개 짜리 신경망

    -> 가로의 깊이냐 세로의 깊이냐

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221006155558513.png" alt="image-20221006155558513" style="zoom: 67%;" />

    - 더 정교한 분할 가능

- 계층적 측징

  - 깊은 신경망에서는 층의 역할이 잘 구분됨

  - 얕은 신경망은 한 두개의 은닉층이 여러 형태의 특징을 모두 담당

    <img src="../../AppData/Roaming/Typora/typora-user-images/image-20221006155807062.png" alt="image-20221006155807062" style="zoom:67%;" />
