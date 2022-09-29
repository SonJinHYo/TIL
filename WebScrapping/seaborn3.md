# Seaborn을 통한 시각화 - 기초3 (사이트의 질문 빈도 실습)

해시코드 사이트에서 어떤 카테고리의 질문이 가장 많은지 시각화



## 1. 스크래핑 라이브러리

```python
from bs4 import BeautifulSoup
import requests
# 가상의 에이전트 생성 라이브러리
from fake_useragent import UserAgent
```





## 2. 스크래핑 후 데이터 전처리

```python
from collections import defaultdict,Counter

# 가상 유저 생성
user_agent = {'User-Agent':UserAgent().random}
# 태그 빈도를 저장할 딕셔너리
frequency = defaultdict(int)

# 1~10페이지 까지의 질문정보를 스크래핑
for i in range(1,11):
    # 페이지의 ul태그 중 question-tags 클래스 안에서, li text들을 가져온다
    res = requests.get(f'https://hashcode.co.kr/?page={i}',user_agent)
    soup = BeautifulSoup(res.text,'html.parser')
	
    ul_tags = soup.find_all('ul','question-tags')
    for ul in ul_tags:
        li_tags = ul.find_all('li')
        for li in li_tags:
            # 태그: 태그갯수
            frequency[li.text.strip()]+=1

# 출력해보기
for k,v in frequency.items():
    print(f'{k} : {v}')
# python : 242
# excel : 2
# pyinstaller : 3
# openpyxl : 4
# kotlin : 7
# android-studio : 9
# game : 1
# polynomial : 1
# pandas : 18
# selenium : 12 등등..

# 상위 10개만 가져와본다
counter = Counter(frequency)

counter.most_common(10)
# [('python', 242),
#  ('c', 42),
#  ('java', 34),
#  ('c++', 23),
#  ('pandas', 18),
#  ('list', 15),
#  ('selenium', 12),
#  ('android', 12),
#  ('for', 10),
#  ('android-studio', 9)]

```





## 3. 그래프로 표현

```python
import matplotlib.pyplot as plt
import seaborn as sns

x = [element[0] for element in counter.most_common(10)]
# ['python', 'c', 'java', 'c++', 'pandas', 'list', 'selenium', 'android', 'for', 'android-studio']
y = [element[1] for element in counter.most_common(10)]
# [242, 42, 34, 23, 18, 15, 12, 12, 10, 9]

plt.figure(figsize=(20,10))
plt.title("Frequency of question in Hashcode")
plt.xlabel('Tag')
plt.ylabel('Frequency')

sns.barplot(x = x,y=y)
```

![image-20220929135418459](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20220929135418459.png)
