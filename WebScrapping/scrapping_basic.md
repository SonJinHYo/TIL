# 웹스크래핑 기초

### 웹 스크래핑 & 웹 크롤링

- 웹스크래핑 : 웹 페이지들로부터 우리가 원하는 정보를 **추출**
  - 특정한 목적으로 특정 웹 페이지에서 데이터를 추출
- 웹크롤링 : 크롤러(로봇)를 이용해서 웹 페이지의 정보를 **인덱싱**
  - url을 타고다니면 반복적으로 데이터를 가져오는 과정



### 올바른 HTTP 요청방법

- 웹스크래핑, 웹크롤링을 통해 **어떤 목적**을 달성하고자 하는가?
  - **가져온 정보를 사용할 때 저작권, 데이터베이스권에 위배되지 않는지 주의한다.**
- 나의 웹 스크래핑/크롤링이 **서버에 영향**을 미치지는 않는가?
  - **요청하고자 하는 서버에 과도한 부하를 주지 않는다.**
- User-Agent 를 알려주기
  - 내가 로봇이 아님을 말해준다.







## BeautifulSoup4

파이썬에서 html코드를 분석해주는 라이브러리

파이썬이 가진 html parser이다.



### BeautifulSoup 객체 만들기

```python
# requests는 페이지에게 request를 보내고 html을 response 받는다.
import requests 
from bs4 import BeautifulSoup

res = requests.get('https://www.naver.com')
>>> 200

# response받은 res의 body를 첫 객체로 넣어주고, html을 분석하므로 html.parser을 넣는다
soup = BeautifulSoup(res.text,'html.parser')
```



- 멋지게 출력 - .prettyfy()
  - `soup.prettyfy()`
  - html 형식이 정리된 형태로 출력된다.

#### 여러 태그 가져오기

```python
# 내용의 head를 가져온다
soup.head
# 내용의 body를 가져온다
soup.body


## 태그

# 가장 첫 h1 태그를 가져온다.
soup.find('h1')
# 모든 h1태그를 가져온다.
soup.find_all('h1')


# 태그이름과 내용 확인
blabla = soup.find('p')
blabla.name
>>> 'p'
blabla.text
>>> 'p태그의 내용'
```



#### for문 활용

```python
# 네이버의 h3 내용들을 찾아보기

res = requests.get('https://www.naver.com')
soup = BeautifulSoup(res.text,'html.parser')

h3_list = soup.find_all('h3')
for h3 in he_list:
    print(h3.text)
```





복잡한 웹사이트를 다루다보면 같은 태그가 많음.

특정 태그의 이름만 써서 가져오면 원치않은 정보를 가져올 때가 많다.

이를 위해 locater을 배움



### Locater

웹사이트를 다루다보면 같은 태그가 많아서 태그만으로는 원치 않은 정보를 불러올 때가 많다. 이를 위해 필요

- Locater : id, class 와 같이 태그를 지칭하는 수단들
  - id : 하나의 고유 태그를 가리키는 라벨
  - class : 여러 태그를 묶는 라벨

```python
import requests
from bs4 import BeautifulSoup

res = requests.get('https://example.python-scrapping.com/')
soup = BeautifulSoup(res.text,'html.parser')

# id로 가져오기 : 특정 태그 하나가 고유하게 가지므로, 찾고 있는 태그 하나가 id를 가진다면 아주 편함
# div 태그의 id가 results라면
soup.find('div',id='results')

# class로 가져오기 : 특정 태그들이 같은 클래스를 가지고 있을 때 사용
# div 태그 중, 'page-header'클래스를 가지고 있는 태그를 찾음
# id 때와 다르게 class= 이라고 붙여줄 필요 없음
find_ph = soup.find("div",'page-header')
find_ph.h1.text
>>> '             Example web scrapping website             '

# 위 처럼 태그 사이의 띄어쓰기, 빈칸이 다 포함되서 나와서 공백들이 포함왜서 나올 수 있다.
find_ph.h1.text.strip()
>>>'Example web scrapping website'
```



```python
import request
from bs4 import BeautifulSoup

# User-Agent를 추가해주기. robot을 거르기 때문에 사람임을 알려준다
user_agent = {'User-Agent':'Mozilaas~~~~'}
res = requests.get('https://hashcode.co.kr',user_agent)

# bs객체를 생성한다.
soup = BeautifulSoup(res.text,'html.parser')

# 객체이기 때문에 한번에 연쇄적으로 찾을 수 있다.

# li태그 중 question-list-item 클래스를 가진 태그 중에서 div태그의 question 클래스를 가진 객체의 텍스트
mini_title = soup.find("li","question-list-item").find("div","question").text
```

### Pagination

- 많은 정보를 인덱스로 구분하는 기법

  ```python
  # Pagination이 되어있는 질문 리스트의 제목을 모두 가져오기
  # 서버에게 부담을 주지 않기위해 1초마다 요청을 보내는 형식으로
  
  import time
  
  for i in range(1,6):
      res = erquests.get("https://hashcode.co.kr/?page{}".format(i),user_agent)
      soup = BeautifulSoup(res.text,"html.parser")
      
      for question in questions:
          print(soup.find("li","question-list-item").find("div","question").text)
  		time.sleep(0.5)
  
  ```

  
