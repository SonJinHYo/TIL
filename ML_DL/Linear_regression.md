# 선형 회귀

- 선형 분류 :  서로 다른 데이터들의 클래스를 분류하는 것. 데이터들의 분포에는 관심이 없음
- 선형 회귀 :  특징 값 간의 상관 관계를 구함. 분류하는 것이 목표가 아니라 데이터 분포에 대한 모델링을 함.



## 선형 회귀 분석 (Linear regression)

~~논문에서 처음으로 그렇게 써서라고 들었는데 정확하진 않다~~

- 데이터들이 선형적인 특징을 가질 때, 대표하는 선형을 구해내는 것

  이것을 데이터들이 구해낸 선형으로 돌아간다고 표현한 것

- ![image-20220927164658106](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20220927164658106.png)

### 학습 방법  :  최소제곱법 (최소 자승법 , Least square method)

- 모델의 예측 값과 데이터 값과의 전체 오차를 최소화하는 기법
  - 오차 : 직선과 데이터간의 y값 차이. 즉, y = ax+b(직선)과 yi(i번째 데이터의 y값)
  - 모든 데이터에 대해 오차들을 각각 제곱하여 합한 식이 최소가 되어야 한다.
    - **(axi+b - yi)^2 들의 합이 최소가 되는 a,b값을 구하는 문제**



 - **a,b 최소를 구하려면?**

   - 위 식을 a,b에 대한 편미분을 했을 때, 0이 되는 값 찾기!

     각 편미분 값이 0이 되는 a,b값을 찾으면 선형회귀문제 해를 구한것

   - a,b에 대한 두 식이 나오게 될 것이므로 연립을 하면 된다.

     -> 행렬 연산으로 해결! 특히 변수가 많아질수록 행렬 연산이 중요해진다.

