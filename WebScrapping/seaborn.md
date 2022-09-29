# Seaborn을 통한 시각화 - 기초

스크래핑의 결과를 그대로 보기엔 쉽지않다.

- seaborn을 통한 데이터 시각화를 한다!



## Seaborn 라이브러리

`seaborn`  : 파이썬의 데이터 시각화 라이브러리. 수려한 그래프를 그릴 수 있음

seaborn 라이브러리를 다운받은 후 `import seaborn as sns` 불러오기



### line Plot

- 꺽은선 그래프

- ```python
  import seaborn as sns
  
  x = [1,3,2,4]
  y= [0.7,0.2,0.1,0.05]
  
  sns.lineplot(x = [1,3,2,4],y= [4,3,2,1])
  ```

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20220929120217124.png" alt="image-20220929120217124" style="zoom:50%;" />



### Bar Plot

- 막대그래프

- 데이터의 '값'과 크기를 직사각형으로 표현

- ```python
  x = [1,3,2,4]
  y= [0.7,0.2,0.1,0.05]
  
  sns.barplot(x = [1,2,3,4],y= [0.7,0.2,0.1,0.05])
  ```

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20220929114425950.png" alt="image-20220929114425950" style="zoom:50%;" />

- ```python
  x = [1,3,2,4]
  y= [0.7,0.2,0.1,0.05]
  
  sns.barplot(x = ['a','b','c','d'],y= [0.7,0.2,0.1,0.05])
  ```

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20220929114530257.png" alt="image-20220929114530257" style="zoom:50%;" />





### Plot의 속성

- `saeborn`은 `matplotlib`을 기반으로 만들어짐
- `matplotlib.pyplot`의 속성을 변경해서 그래프의 요소를 변경/추가가 가능

```python
import matplotlib.pyplot as plt

x = [1,3,2,4]
y= [0.7,0.2,0.1,0.05]
sns.barplot(x = ['a','b','c','d'],y= [0.7,0.2,0.1,0.05])

# 제목 추가
plt.title("Bar Plot")

# x,y축설명
plt.xlabel("X Label")
plt.ylabel("Y Label")

# 그래프 표현
plt.show()
```

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20220929120449791.png" alt="image-20220929120449791" style="zoom:50%;" />



```python
# 그래프의 격자크기를 지정. 그리려는 그래프보다 앞에 넣어줘야한다.
plt.figure(figsize = (20,10))

sns.lineplot(x = [1,3,2,4],y= [4,3,2,1])

# 그래프 표현범위 지정
# y축을 2~3까지만 
plt.ylim(2,3)

# 아까 처음의 lineplot과 같지만 일부분을 확대/축소 한듯한 효과를 준다.
plt.show()
```

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20220929120238462.png" alt="image-20220929120238462" style="zoom: 80%;" />



###### 이후 판다스의 DataFrame과 관련한 함수들은 Pandas와 함께 설명

