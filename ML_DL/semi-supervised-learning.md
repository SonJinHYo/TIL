# 준지도학습, 전이학습

- 현대 머신러닝에서 중요한 연구 주제
- 불완전한 (레이블 정보가 없는) 데이터가 지닌 원천적 성질을 잘 이용해야 함
- 표현 학습은 이런 성질을 **자동으로 알아내려는 시도**, 준지도 학습과 전이 학습의 토대가 된다.



## 표현 학습의 중요성

표현의 중요성 : 692,688 - 숫자크기에 유리한 표현 / (2^4 * 3 * 14431) : 약수를 구할 때 유리한 표현

<img src="../../AppData/Roaming/Typora/typora-user-images/image-20221101104052056.png" alt="image-20221101104052056" style="zoom:50%;" />

### 내부 표현의 이해

- 표현의 가시화

  - 블랙박스로 간주되던 신경망의 내부를 가시화. -> 성능에 대한 통찰력을 얻고, 구조나 하이퍼파라미터를 최적화할 때 필요
  - 준지도 or 전이 학습 설계의 길잡이
  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221101104325001.png" alt="image-20221101104325001" style="zoom:50%;" />

- 신경망 내부 가시화의 여러가지 방법

  - 필터 가시화

    - 첫번째 컨볼루션 층 필터

      <img src="../../AppData/Roaming/Typora/typora-user-images/image-20221101104503082.png" alt="image-20221101104503082" style="zoom:50%;" />

    - 엣지나 블롭이 주로 보임. (영상 종류와 관련없이 일반적인 현상)

  - 특징 맵 가시화

    - 가시화 도구 : 층과 특징 맵을 마우스로 쉽게 선택 가능

    - 아래 그림에서, 녹색 맵에서 고양이 얼굴이 활성화되는게 보임

      ![image-20221101104903998](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221101104903998.png)

  - 역투영 가시화

    - 가시화의 두 가지 방식

      - 앞의 두 가지 가시화 기법은 전방 계산 과정에서 발생하는 필터 or 특징맵을 보여줌
      - 역투영 기법은 특정 노드(뉴런)을 활성화하는 입력 공간의 신호를 알아내어 보여줌

    - 최적화를 이용한 역투영

      - 관찰 대상 노드를 i라 하고, a<sub>i</sub>(**x**)를 영상 **x**가 입력되었을 때 i의 활성값이라 하면

        1. **x_hat** = argmax a<sub>i</sub>(**x**)

        2. 위 식의 최적화 문제를 경사상승법으로(x<sub>0</sub>는 난수초기화) 

           - $$
             del = \eta \frac{\partial ai(x)}{\partial x}
             $$

           - x<sub>t+1</sub> = x<sub>t</sub> + del

        3. 실제로는 여러가지 규제 기법 적용 (R : 규제함수)

           - x<sub>t+1</sub> = R ( x<sub>t</sub> + del )

    - 디컨볼루션을 이용한 역투영

      - 입력영상 **I**를 주고, 관찰 대상 노드 i를 설정하면, i가 속한 층에서 출발해서 역연산으로 **I<sup>-1</sup>**을 보여줌

        <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221101110616294.png" alt="image-20221101110616294" style="zoom:67%;" />

## 준지도 학습



### 동기와 원리

- 레이블이 없는 데이터가 정말 도움이 되는가 -> 그럴수도, 아닐수도 있다. -> **적합한 모델을 찾는 경우 성능 향상**



### 알고리즘

#### 현대적 생성 모델

- 생성모델 GAN을 사용

- 가짜 샘플에 해당하는 c+1 이라는 레이블을 추가로 사용

- 분별기 D의 목적함수는 세 가지 항을 가짐

  - 가짜 샘플을 c+1에 배정하는 항

  - **X<sub>u</sub>** 샘플이 c+1에 배정되는 것을 막는 항

  - **X<sub>l</sub>** 샘플을 해당 부류로 배정하는 항

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221101112455052.png" alt="image-20221101112455052" style="zoom:67%;" />

#### self-learning (자가 학습)

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221101145307296.png" alt="image-20221101145307296" style="zoom:80%;" />

- 애매한 샘플에 대해 민감한 상황 발생



#### co-training (협동 학습)

- 학습기 2개가 서로 협동하여 **X<sub>expand</sub>**를 확장하면서 발전해 감

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221101150610725.png" alt="image-20221101150610725" style="zoom:80%;" />

