# matplotlib workflow(시각화 과정) 정리

###### 출저 : 2022 pycon - 혼란한 Matplotlib에서 질서 찾기 (이제현)

### 전체 요약

![image-20221017141220947](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221017141220947.png)





###### 흔한 오해 : 내 그래프가 별로인 이유는?

1. Matplotlib이 문제다
   - seaborn, plotly 등 다른 라이브러리를 사용
2. Python이 문제다
   - 다른 언어(R 등)를 써야겠다

Matplotlib으로 대부분의 그래프 표현이 깔끔하고 이쁘게 가능!



## 구조

![image-20221017141821951](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221017141821951.png)

- matplotlib은 Numpy를 기반으로 한다
- matplotlib은 여러 라이브러리를 포함하는 **생태계**





## 시각화 순서



### 1. seabron을 통한 시각화 환경 설정 (채색, 스타일)

- matplotlib으로 설정하려면 손이 많이가기 때문

- ```python
  # 그냥 사용한 경우
  x,y = df_peng['~~'], df_peng['~~']
  plt.scatter(x,y)
  
  # seaborn을 통한 환경 설정 후 
  
  sns.set_context("talk") # 구성요서 배율 설정
  sns.setpalette("Set2") # 배색 설정
  sns.set_style("whitegrid") # 테두리 설정
  
  plt. scatter(x,y)
  ```

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221017142449959.png" alt="image-20221017142449959" style="zoom: 50%;" />





### 2. matplotlib을 통해 화면 구성 설정

#### 중요 개념 : plt? ax?

- plt : 상태 기반 명령어. 코드가 짜여진 대로 격자를 수정해나간다

  - 객체 지향적이지 않음

  - <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221017142944471.png" alt="image-20221017142944471" style="zoom:50%;" />

  - 코딩하다보면 위아래로 왔다갔다 하게 되고 나열식 코드가 됨

    

- ax : 객체 지향 기반 명령어. 격자를 객체로 지정하는 방법





1. 레이아웃 사전 설정 : `fig,axs = plt.subplot(ncols=2,figsize=(8,4),....)` 

   - 각 subplot이 axs가 된다

2. 대상 지정 시각화 후, 공통 부분은 for 문을 통해 객체 지향 특성을 활용 가능

   - ```python
     # 공통 부분이 아닌건 따로
     axs[0].plot(x,power,...)
     axs[1].plot(x,torque, ...)
     
     # 공통 부분은 for문으로
     for ax in axs:
         ax.set_xlabel('time')
         ax.legend()
         
     # 공통 부분이 아닌건 따로
     axs[0].set_ylabel('output')
     
     # 전체 격자 설정 따로
     fig.subtitle("performance")
     ```

### 객체 지향 방식

![image-20221017143610847](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221017143610847.png)

#### 속성

- 선 객체 속성
  - c : color
  - lw : line width
  - ls : line style
  - alpha 등등...
- 면 객체 속성
  - fc : face color
  - ec : edge color
  - lw : line width
  - ls : line style
  - alpha 등등...
- 문자 객체 속성
  - font name, font style, font size 등등 font가 들어간 이름
  - 그 외에 color, rotation , size 등

- 속성 객체 추출,제어 등등 예시

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221017143931782.png" alt="image-20221017143931782" style="zoom:50%;" />

  - 속성 추출 : 객체.get_속성()
    - `ax.collections[0].get_fc()`
    - `ax.collections[0].get_sizes()`
    - `ax.lines[0].get_c()`  등등
  - 속성 제어 : 객체.set_속성()
    - `ax.collections[0].get_fc("cornflowerblue")`
    - `ax.collections[0].get_sizes([100])`
    - `ax.lines[0].get_c("#00FF00")`  등등





### 3~5. 펭귄데이터에서 중요데이터 강조까지

```python
## seaborn을 통한 환경 설정
fig,ax = plt.subplots()
sns.violinplot(x='species',y='body_mass_g',data = df.peng,hue= 'sex', split=True, ax= ax)
ax.set(xlabel='',ylabel='',title='Body mass of penguins (g)')
```

<img src="../../AppData/Roaming/Typora/typora-user-images/image-20221017144735262.png" alt="image-20221017144735262" style="zoom:50%;" />

```python
# 범례 자리가 없으니 아래쪽 공간 추가 확보
ax.set_ylim(ymin=2000)
```

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221017145110390.png" alt="image-20221017145110390" style="zoom:50%;" />

###### data ink ration : 데이터를 나타내기 위해 사용되는 잉크 양 / 전체 잉크 총량. 이 비율이 높을수록 좋은 그래프이다.



```python
## data ink ratio를 높이기 위해 테두리와 눈금을 제거해준다
ax.spines[["left","top","right"]].set_visible(False)
ax.tick_param(axis="y",length=0)

# 시각화가 잘 되도록 부가요소 설정. y축 기준으로 밝은회색선 설정
ax.grid(axis="y",c = "lightgray")
```

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221017145810854.png" alt="image-20221017145810854"  />

```python
## 부가요소를 설정하여 강조를 해준다

# 비중요 데이터들을 약하게 표시
for i,obj in enumerate(ax.collections):
    ec = "gray" if i<6 else "k"
    lw = 0.5 if i<6 else "k"
    obj.set_ec(ec)
    obj.set_lw(lw)
    
    # 모두 중간값을 키워서 강조. 
    if (i+1)%3 == 0:
        obj.set_sizes([60])
    # 비중요 데이터 흐리게
	if i<6:
        obj.set_fc(set_hls(onj.get_fc(),ds = -0.3, dl = 0.2))
        
        
# 테두리와 박스라인이 비슷하다. box_line을 강조
for i,line in enumerate(ax.lines):
    if i>3:
        line.set_color("k")

# Legend를 새로 만들 준비
hadles = ax.collections[-3:-1] 
labels = ["Male","Female"]

# legend 생성
ax.legend(handels, labels, fontsize=14, title="sex", title_fontsize=14, edgecolor = "lightgray", loc = "lower right")
```

![image-20221017150754043](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221017150754043.png)

 



### 6. 보조 요소 설정

- 정성적 전달력 강화
- 도형 객체 삽입 : `Axes.add_artist()`, etc
  - 데이터의 의미 설명, 데이터간 관계 표현
  - plot으로 부족한 표현력 보완

- 예시

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221017151408939.png" alt="image-20221017151408939" style="zoom:80%;" />



## 정리

**상태 기반 방식**

- 장점

  - 간단하고 빠르게 형상만 확인하기에 유리

- 단점

  - 논리적 흐름이 아닌 시간 흐름에 따른 코딩

  - 그리기의 한계를 벗어나지 못함



**객체 지향 방식**

- 장점 
  - 논리에 따른 코딩 : 재사용 & 유지보수에 유리
  - Matplotlib 생태계 활용 가능
    1. Matplotlib 호환 라이브러리로 작성
    2. 객체 제어 : 강조 등 분석가 의도 반영
