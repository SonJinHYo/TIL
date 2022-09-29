# Seaborn을 통한 시각화 - 기초2 (날씨정보 실습)



기상청의 오늘의 기온을 스크래핑해서 나타내보기



기상청 오늘 날씨 : https://www.weather.go.kr/w/index.do

## 1.  **스크래핑 라이브러리**

```python
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
import time
```


## 2. **스크래핑을 한 뒤 데이터 전처리**

```py
# 크롬 드라이버 저장
driver = webdriver.Chrome(service = Service(ChromeDriverManager().install()))

# 기온 정보를 가져와서
driver.get('https://www.weather.go.kr/w/index.do')
time.sleep(2)
temps = driver.find_element(By.ID,'my-tchart').text
# 24℃\n25℃\n25℃\n25℃\n24℃\n22℃\n20℃\n19℃\n19℃\n18℃\n18℃\n17℃\n17℃\n17℃\n16℃\n16℃\n16℃\n16℃\n16℃\n17℃\n20℃'

# int값 리스트로 저장
temps = list(map(int,temps.replace('℃','').split('\n')))
# [26, 27, 27, 27, 27, 25, 23, 22, 21, 20, 20, 19, 18, 18, 18, 17, 17, 17, 17, 17, 20]

# 스크래핑한 기온들 중 첫 기온의 시각
start_time = int(driver.find_element(By.XPATH,f'//*[@id="digital-forecast"]/div[1]/div[3]/div[2]/div[1]/div[1]/div/div[2]/ul[1]/li[1]/span[2]').text[:-1])

# 시각 리스트
times = [i%24 for i in range(start_time,start_time+len(temps))]
```





## 3. 그래프로 표현

```python
import seaborn as sns
# 시간과 온도는 잘 받았지만 두 가지 문제 발생
# 1. 자동 정렬

# times : [13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
# temps : [24, 25, 25, 25, 24, 22, 20, 19, 19, 18, 18, 17, 17, 17, 16, 16, 16, 16, 16, 17, 20]
sns.lineplot(
    x = times,
    y = temps,
)
```

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20220929132039013.png" alt="image-20220929132039013" style="zoom:50%;" />

```python
# 2. 정렬을 꺼도 x값이 0에서 시작해서 끊김

sns.lineplot(
    x = times,
    y = temps,
    sort=False
)
```

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20220929132112117.png" alt="image-20220929132112117" style="zoom:50%;" />

```python
# 원활한 표기를 위해 우선은 온도만 표시

sns.lineplot(
    x = [i for i in range(len(temps))],
    y = temps,
    sort=False
)
```

![image-20220929132134969](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20220929132134969.png)

```python
# 좀 더 느슨한 그래프로 만들기 -> ylim 조정

import matplotlib.pyplot as plt

plt.ylim(min(temps)-3, max(temps)+3)

sns.lineplot(
    x = [i for i in range(len(temps))],
    y = temps,
    sort=False
)
```

![image-20220929132151785](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20220929132151785.png)
