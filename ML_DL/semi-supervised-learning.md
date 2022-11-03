# 준지도학습, 전이학습

- 현대 머신러닝에서 중요한 연구 주제
- 불완전한 (레이블 정보가 없는) 데이터가 지닌 원천적 성질을 잘 이용해야 함
- 표현 학습은 이런 성질을 **자동으로 알아내려는 시도**, 준지도 학습과 전이 학습의 토대가 된다.
  - **준지도 학습의 주제** : 레이블이 없는 샘플을 버릴 것인가, 적절히 이용하는 알고리즘을 고안할 것인가
  - **전이 학습의 주제** : 도메인 자체가 다른 상황에서, 많은 양의 데이터를 다시 수집? 아니면 조금만 더 수집하고 분류기를 미세조정할 것인가?



## 표현 학습의 중요성

표현의 중요성 : 692,688 - 숫자크기에 유리한 표현 / (2^4 * 3 * 14431) : 약수를 구할 때 유리한 표현

<img src="../../AppData/Roaming/Typora/typora-user-images/image-20221101104052056.png" alt="image-20221101104052056" style="zoom:50%;" />

### 내부 표현의 이해

- 표현의 가시화

  - 블랙박스로 간주되던 신경망의 내부를 가시화. -> 성능에 대한 통찰력을 얻고, 구조나 하이퍼파라미터를 최적화할 때 필요

  - 준지도 or 전이 학습 설계의 길잡이

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221101153726996.png" alt="image-20221101153726996" style="zoom:80%;" />

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

    - 디컨볼루션을 이용한 역투영 (개념만)

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

- 협동이 아닌 2개가 서로 가르치는 방식으로 수정하면

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221103150018526.png" alt="image-20221103150018526" style="zoom:67%;" />

#### 그래프 방법

- 샘플 사이의 유사도에 따라 그래프를 구성
  - ex. 샘플마다 k개의 최근접 이웃을 찾아 edge로 이어줌
  - 복잡한 비선형 분포를 반영하기 위해 정교한 그래프 구축 방법이 필요
- 최소 분할을 적용하여 분할선을 찾음
- 같은 부분집합에 속하는 샘플에 같은 부류 레이블을 부여

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221103150359735.png" alt="image-20221103150359735" style="zoom:67%;" />

#### 밀집 지역 회피

- 결정 경계가 밀집 지역을 지나면 오분류 가능성 높아짐

  =>밀집 지역을 회피하여 결정 경계를 정함

- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221103150551636.png" alt="image-20221103150551636" style="zoom:80%;" />



## 전이 학습

- 일상에서의 전이 학습
  - 피아노를 칠 줄 아는 사람은 못 치는 사람보다 바이올린을 빨리 배운다
  - c언어를 잘 다루면 C++도 쉽게 잘 다룬다.
- 기계 학습에서 전이 학습
  - 어떤 도메인에서 제작한 프로그램을, 데이터가 적어 애를 먹는 새로운 도메인에 적용하여 높은 성능을 얻는 기법
  - 현대 기계 학습에서 널리 활용

#### 과업이 다른 경우 , 도메인이 다른 경우

과업이 달라지는 경우 

- 영상 인식에서 음성 인식으로 전이.
  - 두 과업 사이의 거리가 아주 먼 상황 (실용적인 연구 결과가 없음)

- 자연 영상을 1000부류로 인식하는 과업을 200종 나뭇잎을 인식하는 과업으로 전이.
  - 과업간 거리가 짧아 좀 더 실용적



도메인이 달라지는 경우: 특징 공간이 다른 경우, 특징 공간은 같지만 데이터의 확률 분포가 다른 경우로 구분

- 특징 공간이 다른 경우 .
  - 영불 번역기를 영한 번역기로 전이
  - 한국어 정보 검색기를 베트남어 정보 검색기로 전이
- 특징 공간은 같지만 데이터의 분포가 다른 경우
  - 한국인이 쓴 필기 숫자 데이터베이스를 다른 나라에서 사용할 때, 나라별로 쓰는 방식의 차이가 있을 수 있음
  - 전이 학습을 잘 해낸다면 소량의 데이터로 좋은 성능을 적용 가능



### 과업 전이

#### 기성 CNN 특징

- 성공적으로 학습된 신경망의 특징 추출 부분을 **다른 과업**에 활용

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221103153811503.png" alt="image-20221103153811503" style="zoom:67%;" />

#### 동결 방식

- 위 그림의 파란색 실선 화살표로 표시된 층 중 하나를 골라 특징을 취한다.
- 이 특징은 컨볼루션 층을 여럿 통과하면서 정제외었으므로 얕은 신경망(MLP같은)에 사용해도 높은 성능으로 분류 가능



<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221103153942713.png" alt="image-20221103153942713" style="zoom:67%;" />

#### 미세 조정 방식

- 위 그림의 FC 부분을 떼어낸 후, 새로운 구조를 덧붙여 다시 학습
- **이 때, 학습률은 낮게 설정**
  - 높으면 원래 가중치가 훼손되어 버림

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221103154051264.png" alt="image-20221103154051264" style="zoom:67%;" />



### 도메인 전이

- 과업은 같은데( =레이블 공간이 같다), 도메인이 다른 상황
- 그 중 특징 공간이 같고 확률 분포가 다른 상황을 **도메인 적응**이라 함



#### 도메인 전이 방법

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221103154912379.png" alt="image-20221103154912379" style="zoom:67%;" />



#### DAUME2009 방법

- 특징 공간을 3배로 확장하여 두 도메인의 확률분포를 맞춤

#### Sun2016 방법

- Daume2009 방법을 비지도 도메인으로 확장
- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221103155543805.png" alt="image-20221103155543805" style="zoom:80%;" />
  - 파란 점은 원천 도메인 샘플, 빨간 점은 목표 도메인 샘플
  - 화이트닝 변환과 컬러링 변환으로 두 도메인의 확률분포를 맞춤
    - 화이트닝 == 정규화

- 알고리즘

  ![image-20221103155648735](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221103155648735.png)
