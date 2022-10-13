# 서포트 벡터 머신 (SVM)

## 선형 SVM 분류

- 서포트 벡터 머신
  - 두 클래스로 부터 최대한 멀리 떨어져 있는 결정 경계를 이용한 분류기
  - 결정 경계 찾기 : 클래스 사이를 지나가는 도로 폭(마진)이 특정 조건하에 최대가 되어야 함

### 라지 마진 분류

- 클래스를 구분하는 가장 넓은 도로
- 분류 대상 클래스들 사이의 가장 큰 도로(라지 마진)을 계산하여 클래스 분류

- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011164354278.png" alt="image-20221011164354278" style="zoom:67%;" />



### 하드 마진 분류

- 모든 훈련 샘플이 도로 바깥쪽에 올바르게 분류되도록 하는 마진 분류
- 훈련 세트가 선형적으로 구분되는 경우에만 가능. 이상치에 민감
  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011164658171.png" alt="image-20221011164658171" style="zoom:67%;" />





### 소프트 마진 분류

- 마진 위반(margine violation) 사례의 발생 정도를 조절하면서 도로의 폭을 최대로 넓게 유지하는 마진 분류
- 마진 위반 : 훈련 샘플이 도로 상에 위치하거나 결정 경계를 넘어 해당 클래스 반대편에 위치하는 샘플
- ex. 붓꽃 품종 이진 분류
  - 사이킷런 SVM 분류기 LinearSVC를 활용 (C를 이용하여 규제)
  - `svm_clf1 = LinearSVC(C=1, loss='hinge', random_state=42)`
  - `svm_clf2 = LinearSVC(C=100, loss='hinge', random_state=42)`
  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221011165036391.png" alt="image-20221011165036391" style="zoom:67%;" />



### 힌지 손실 함수

- **힌지 손실 l(y) =  max(0, 1 - t * y) **
  - t : 타겟 , y : 결정함수
- 제곱 힌지 : l(y)**2, 마진의 영향을 높인다 









## 비선형 분류



### 1. 선형 SVM 활용

- 특성 x<sub>1</sub> 하나만 갖는 모델에 새로운 특성 x<sub>1</sub><sup>2</sup>을 추가한 후 선형SVM 적용
  - 2차 다항식 모델 : y = w<sub>2</sub>x<sub>1</sub><sup>2</sup> + w<sub>1</sub>x<sub>1</sub> +w<sub>0</sub>
- 만약 moons데이터셋 이라면? (반원 모양의 클래스) => 3차 다항식



### 2. SVC + 커널 트릭

#### 커널 트릭

- 어떠한 특성도 새로 추가하지 않으면서 특성을 추가한 것과 수학적으로 동일한 결과가 나오게 하는 기법

- 다항 커널 : 다항 특성을 추가하는 효과를 주는 함수

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221013104835275.png" alt="image-20221013104835275" style="zoom: 80%;" />

- 가우시안 RBF 커널 : 유사도 특성을 추가하는 효과를 내주는 함수



#### 적절한 하이퍼파라미터 선택

- 모델이 과대적합 => 차수를 낮춰야함
- 적절한 하이퍼파라미터는 **그리그 탐색 등** 을 이용하여 찾는다
  - 그리드 폭을 크게에서 세밀한 방향으로
- 하이퍼파라미터의 역할을 잘 파악하고 있어야 함




### 유사도 특성



#### 유사도 특성 + 선형 SVM

- 유사도 함수 : 각 샘플에 대해 특정 랜드마크와의 유사도를 측정하는 함수

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221013105200882.png" alt="image-20221013105200882" style="zoom:80%;" />

- <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221013105432659.png" alt="image-20221013105432659" style="zoom: 67%;" />

- 유사도 함수 적용의 장단점

  - 각 샘플을 랜드마크로 지정후 유사도 특성을 추가
    - n개의 특성을 가진 m개의 샘플  =>  m개의 특성을 가진 m개의 샘플
  - 장점 : 차원이 커지면서 선형적으로 구분될 가능성이 높아짐
  - 단점 : 훈련 세트가 매우 클 경우, 동일한 크기의 특성이 아주 많이 생성되어 버림



#### 추천 커널

- SCV kernel의 디폴트값은 rbf kernel : 대부분의 경우 이 커널이 잘 맞늠
- 선형 모델이 예상되는 경우 linear  kernel
  - 훈련 세트가  크거나 특성이 아주 많을 경우 Linear SVC가 빠름
- **시간과 컴퓨터 성능이 허락해준다면** 교차 검증, 그리드 탐색으로 적절한 커널을 찾기
- 훈련 세트에 특화된 커널이 알려져 있다면 해당 커널을 사용



#### 계산 복잡도

![image-20221013105910747](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221013105910747.png)







## SVM 회귀

- SVM 분류 목표 : 마진 위반 발생 정도를 조절하면서 두 클래스 사이의 도로폭을 최대한 넓게 하기
- SVM 회귀 목표 : 마진 위반 발생 정도를 조절하면서 도로폭을 최대한 넓혀서 **도로 위에 가능한 많은 샘플 포함시키기**
- 회귀 모델의 마진 위반 사례 : 도로 밖의 샘플



### 선형 SVM 회귀

선형 회귀 모델을 SVM을 이요하여 구현

```python
from sklearn.svm import LinearSVR

svm_reg = Linear(epsilon = e)
```

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221013110207099.png" alt="image-20221013110207099" style="zoom: 80%;" />





### 비선형 SVM 회귀

커널 트릭을 활용하여 비선형 회귀 모델 구현

```python
from sklearn.svm import SVR

svm_poly_reg = SVR(kernel = 'poly', degree=d , C=C , epsilon = e , gamma = 'scale')
```

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221013110337600.png" alt="image-20221013110337600" style="zoom:80%;" />



## SVM 이론



### 선형 SVM 작동 원리

####  결정 함수와 예측

- 선형 SVM 분류기 모델의 결정 함수
  - h(x) = **w**<sup>T</sup>**x** + b

- 선형 SVM 분류기의 예측
  - y_hat = 0 if h(x) < 0 else 1
- 결정 경계
  - 결정 함수의 값이 0인 점들의 집합



#### 목적 함수

- 결정 함수의 기울기와 마진 폭

  - 결정 함수의 기울기가 작아질 수록 마진 폭이 커짐

  - 결정 함수의 기울기는 ||w||에 비례한다

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221013110855880.png" alt="image-20221013110855880" style="zoom:67%;" />

  - 마진을 크게 하기 위해선 ||w||을 최소화 해야함

    - 하드 마진 : 모든 양성(음성) 샘플에 대한 결정함수의 값이 1(-1) 보다 크다(작다)
    - 소프트 마진 : 모든 샘플에 대한 결정 함수의 값이 지정된 값 이상 또는 이하이어야 한다

##### 하드 마진 선형 SVM 분류기의 목적 함수

- 목적 함수 : (1/2)**w**<sup>T</sup>**w**
- 다음 조건 하에서 목적 함수를 최소화 시키는 **w**,b를 구해야함
  - t<sup>(i)</sup> (**w**<sup>T</sup>**x**<sup>(i)</sup>  +b) >= 1
    - x<sup>(i)</sup> : i번째 양성 샘플
    - t<sup>(i)</sup> : 양성 샘플일 때 1, 음성일 때 -1



##### 소프트 마진 선형 SVM 분류기의 목적 함수

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221013111406126.png" alt="image-20221013111406126" style="zoom:80%;" />
