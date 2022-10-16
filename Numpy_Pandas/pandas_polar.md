# matplotlib으로 pandas데이터 육각형그래프로 표현하는 법

###### 데이터 출저 : kaggle - Marvel Superheroes

```python
## 판다스로 가져온 히어로 데이터 (히어로마다 능력치가 있다.)
heros = stats[-9:]
heros.drop(columns=['Alignment','Total'],inplace=True)
heros.reset_index(drop=False,inplace=True)


## 능력치 부분만 골라서 라벨링(인덱스와 이름을 제외)
label = heros.columns[2:]
label_len = len(label)

## 각도 구현
angels = [x/label_len*2*np.pi for x in range(label_len)]
## 원형그래프이므로 처음과 끝을 연결. 리스트가 아니므로 슬라이싱으로 더한다
angels += angels[:1]

## 색 차별화
palette = plt.cm.get_cmap("Set2",len(heros.index))


## 그래프 셋팅
fig = plt.figure(figsize=(30,30))
fig.set_facecolor('white')



## 여러 그래프 그리기
for i,row in heros.iterrows():
    color = palette(i)
    # 행데이터의 첫번째 열(이름)부터해서, 이름을 드랍하고 리스트화
    data = heros.iloc[i][1:].drop('Name').tolist()
    # 원형그래프이므로 처음과 끝을 연결
    data += data[:1]
    
    # ploar = True : 원형
    ax = plt.subplot(3,3,i+1,polar=True)
    # 시작 각도 설정
    ax.set_theta_offset(np.pi/2)
    # 방향 설정 (시계방향)
    ax.set_theta_direction(-1)
    
    # 각도값(x) 셋팅
    plt.xticks(angels[:-1],label,fontsize=13)
    ax.tick_params(axis='x',which='major',pad=15)
    # 반지름값(r(==y)) 셋팅
    ax.set_rlabel_position(0)
    # 능력치 범위가 0~100
    plt.yticks([0,20,40,60,80,100],fontsize=10)
    plt.ylim(0,100)
    
    # 그래프 표현(각도,반지름,색상,라인길이,라인스타일)
    ax.plot(angels,data,color=color,linewidth=2,linestyle='solid')
    # 그래프 색 채우기
    ax.fill(angels,data,color=color,alpha=0.4)
    
    # 타이틀 설정
    plt.title(row.Name,size=40,color=color,x=-0.2,y=1.2,ha='left')
    
# 그래프간 간격 설정
plt.tight_layout(pad = 5)
plt.show()
```

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221016204110126.png" alt="image-20221016204110126" style="zoom:200%;" />
