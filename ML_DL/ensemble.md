# 앙상블 학습, 랜덤포레스트

- 앙상블 : 여러 개의 예측기로 이루어지 그룹
- 앙상블 학습 : 예측기 여러 개의 결과를 종합하여 예측값을 지정하는 학습
- 앙상블 기법 : 앙상블 학습을 지원하는 앙상블 학습 알고리즘



## 투표식 분류기

동일한 훈련 세트에 대해 여러 종류의 분류기를 이용, 이 때 분류기마다 앙상블 학습을 적용 후 직간접 투표를 통해 예측값 결정

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221020103103208.png" alt="image-20221020103103208" style="zoom:67%;" />

- 직접 투표
  - 앙상블에 포함된 예측기들의 예측값들에서, 다수인 것으로 예측값을 결정
  - 가장 성능이 높은 모델보다 투표결과가 더 좋게 나옴
- 간접투표
  - 앙상블에 포함된 예측기들의 예측값들에서, 확률값들의 평균값으로 예측값을 결정
  - 전제 : 모든 예측기다 `predict_porba() `메서드와 같은 확률 예측 기능을 지원해야함
  - 높은 확률에 보다 비중을 두기 때무에 직접투표 이상의 성능을 가짐



### 투표식 분류기 특징

- 앙상블에 포함된 분류기들 사이의 독립성이 전재되는 경우, 개별 분류기 보다 정확한 예측 가능
- 단, 독립성이 보장되지 못한다면 투표식 분류기의 성능이 더 낮아질 수 있음



### 투표식 분류기 예제 ( 사이킷런 )

- `VotingClassifier` : 투표식 분류기 모델 제공

- `voting = 'hard'` : 직접 투표 방식 지정 하이퍼파라미터

- `voting = 'soft'` : 간접 투표 방식 지정 하이퍼파라미터

  - **주의 사항** : SVC모델을 지정할 때, `probability = True`를 사용해야 `predict_proba()` 메서드가 지원됨

- 예시

  - ```python
    # 선형회귀, 랜덤포레스트, 서포트벡터머신
    log_clf = LogisticRegression(solver='lbfgs',random_state=42)
    rnd_clf = RandomForestClassifier(n_estimation=100, random_state=42)
    svm_clf = SVC(gamma='scale', randmom_state = 42)
    
    # 투표식 분류기 (직접 투표)
    voting_clf = VotingClassifier(estimators=[('lr',log_clf),('rf',rnd_clf),('svc',svm_clf)], voting='hard')
    
    # 학습
    voting_clf.fit(X_train,y_train)
    ```





## 배깅 ( bootstrap aggregation), 페이스팅

- 동일한 예측기를 훈련 세트의 다양한 부분집합을 대상으로 학습시키는 방식
- 학습에 사용되는 부분집합에 따라 훈련세트가 다른 예측기를 학습시키는 앙상블 학습 기법
- 부분집합을 임의로 선택할 때, **중복 허용 여부**에 따라 학습 방식이 달라짐
  - 배깅 : 중복 허용 샘플링
  - 페이스팅 : 중복 미허용 샘플링

- 비깅 주의사항
  - 통계 분야에서 사용됨. **리샘플링**이라고도 불림
  - 배깅 방식만은 동일 훈련 샘플을 여러번 샘플링할 수 있음



### 배깅/페이스팅 예측 방식

- 개별 예측기의 결과를 종합해서 최종 예측값 지정
- 분류 모델 : 직접 투표방식 사용. 예측값들 중 **최빈값** 선택
- 회귀 모델 : 수집된 예측값의 **평균값** 선택



### 앙상블 학습의 편향과 분산

- 개별 예측기의 경우에 비해 편향은 비슷, 분산은 줄어듬. 이로인한 **과대적합의 위험성 감소**

- 개별 예측기 : 배깅/페이스팅 방식으로 학습하면 전체 훈련 세트를 대상으로 학습한 경우에 비해 편향이 커짐. **과소적합 위험성 증가**

  ![image-20221020105206279](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221020105206279.png)

### 사이킷런의 배깅/ 페이스팅

- 분류 모델 : `BaggingClassifier`

- 회귀 모델 : `BaggingRegressor`

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221020105344531.png" alt="image-20221020105344531" style="zoom:50%;" />

#### 배깅 vs 페이스팅

- 배깅은 평향 증가, 분산 감소
  - 표본 샘플링의 무작위성으로 인해 예측기들 사이의 상관관계가 약화되어서
- 전반적으로 배깅 방식이 좀 더 나은 모델을 생성함
- 교차검증을 통해 확인 필요



#### obb 평가

- out of bag 샘플 : 선택되지 않은 훈련샘플
- obb샘플을 활용하여 앙상블 학습에 사용된 개별 예측기의 성능 평가 가능
- `BaggingClassifier`에서 `obb_score=True`로 설정하면 자동으로 obb평가 실행





## 랜덤 패치와 랜덤 서브스페이스

- `BaggingClassifier`은 **특성을 대상으로 하는 샘플링 기능 지원**
  - `max_features`
    - 학습에 사용할 특성 수 지정
    - 특성 선택은 무작위
      - 정수인 경우 : 지정된 수 만큼 특성 선택
      - 부동소수점( [0,1] ) 인 경우 :  지정된 비율만큼 특성 선택
    - max_samples와 유사 기능 수행
  - `bootstrap_featrues`
    - 학습에 사용할 특성을 선택할 때 중복 허용 여부 지정
    - default=False
    - bootstrap과 유사 기능 수행
- 이미지 등과 같이 매우 높은 차원데이터셋을 다룰 때 유용
- 더 다양한 예측기를 만들며, 편향이 커지지만 분산이 낮아짐





### 랜덤 패치 기법

- 훈련 샘플과 훈련 특성 모두를 대상으로 중복을 허용하며, 임의의 샘플,특성 각각의 수만큼 샘플링해서 학습하는 기법

- 전제 조건

  1. `bootstrap = True` or `max_samples<1.0`

  2. `bootstrap_features=True` or` max_features<1.0`



### 랜덤 서브스페이스 기법

- 전체 훈련 세트를 학습 대상으로 삼지만 훈련 특성은 임의의 특성 수만큼 샘플링해서 학습하는 기법

- 전제 조건

  1. `bootstrap = False` or `max_samples=1.0`

  2. `bootstrap_features=True` or` max_features<1.0`





## 랜덤 포레스트

- 배깅/페이스팅 방법을 적용한 의사결정트리의 앙상블을 최적화한 모델
- 분류 : `RandomForestClassifier`
- 회귀 : `RandomForestRegressor`



### 엑스트라 트리

- `ExtraTreesClassifier`

- **랜덤포레스트의 마디 분할 방식**

  - 특성 : 무작위

  - 특성 임계값 : 무작위로 분할한 다음 최적 분할값 선택



- **엑스트라트리의 마디 분할 방식**

  - 특성과 특성임계값 모두 무작위

  - 일반적인 랜덤포레스트보다 **속도가 훨씬 빠름**

  - 편향은 증가, 분산 감소



### 특성 중요도

- 해당 특성을 사용한 마디가 평균적으로 불순도를 얼마나 감소시키는지를 측정
  - 불순도를 많이 줄이면 그만큼 중요도가 커짐
- 사이킷런의 `RandomForestClassifier` 훈련이 끝난 뒤 특성별 중요도의 합이 1이 되도록 하는 방식
  - 특성별 상대적 중요도를 측정한 후 `feature_importances_ ` 속성에 저장





## 부스팅

- 성능이 약한 학습기들을 선형으로 여러 개 연결하여 강한 성능의 학습기를 만드는 앙상블 기법
  - 순차적으로 이전 학습기의 결과를 바탕으로 성능을 조금씩 높혀가는 방식
  - 순차적으로 학습하기에 배깅/페이스팅에 비해 확장성이 떨어짐
- 기본 아이디어 : 성능이 약한 예측기의 단점을 보완하여 좋은 성능의 예측기를 훈련해 나가는 것



### 에이다부스트( AdaBoost)

- 좀 더 나은 예측기를 생성하기 위해 잘못 적용된 가중치를 조정하여 새로운 예측기를 추가하는 앙상블 기법

- 이전 모델이 제대로 학습하지 못한 (과소적합된) 샘플들에 대해 가중치를 더 높이는 방시으로 새로운 모델 생성

- 새로운 예측기는 학습하기 어려운 샘플에 조금씩 더 잘 적응하는 모델이 **연속적으로 만들어져감** (=병렬처리 어려움)

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221020111403489.png" alt="image-20221020111403489" style="zoom:67%;" />

- 분류,회귀모델 : `AdaBoostClassifier` , `AdaBoostRegressor`





### 그레이디언트부스팅

- 이전 학습기에 의한 오차를 보정하도록 새로운 예측기를 순차적으로 추가 (아이디어는 에이다와 비슷)
- 샘플의 가중치를 수정 X , 이전 예측기가 만듬 오차에 대한 새로운 예측기를 학습시킴
- 분류, 회귀모델 : `GradientBoostingClassifier` , `GradientBoostingRegressor`
  - 랜덤포레스트와 비슷한 하이퍼파라미터를 제공



#### 그레이디언트 부스티드 회귀 나무 (GBRT)

그레이디언트 부스팅(회귀) + 의사결정나무

- 예제 : 2차 다항식 데이터셋에 의사결정나무 3개를 적용한 효과와 동일하게 작용

- `grbt = GradientBoostingRegressor(max_depth=2,n_estimators=3, learning_rate=1.0)` , `grbt.fit(X,y)`

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221020112127059.png" alt="image-20221020112127059" style="zoom:80%;" />

- learning_rate(학습률, 트리의 기여도)
  - learning_rate : 각 의사결정나무의 기여도 조절에 사용(이전의 학습률과는 다름)
  - 축소 규제 : 학습률을 낮게 정하면 많은 수의 의사결정나무가 필요하지만 성능 증가 (과대적합 주의)
  - 이전 의사결정나무에서 학습된 값을 전달할 때 사용되는 비율
    - 1.0 : 그대로 전달
    - 1.0 이하 : 해당 비율만큼만 전달



#### 최적의 의사결정나무 수 확인법

- 조기종료 기법 활용 
  - `stage_predict()` : 많은 수를 훈련한 후 최적의 수 찾기
  - `warm_start=True` : 기존 트리 유지하고 훈련 추가



#### 확률적 그레이디언트 부스팅

- 각 의사결정나무가 훈련에 사용할 훈련 샘플의 비율을 지정하여 학습
- 속도 빠름
- 편향 증가, 분산 감소



#### XGBoost : 최적화된 그레이디언트부스팅

- Extreme Gradient Boosting
- 빠른 속도, 확장성과 이식성이 뛰어남
- 사이킷런과 비슷한 API
- 조기종료등 다양한 기능 제공
