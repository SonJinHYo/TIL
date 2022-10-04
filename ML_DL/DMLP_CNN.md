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
