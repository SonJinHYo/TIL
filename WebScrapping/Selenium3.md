# Selenium 기초(3) - 마우스, 키보드 조작 / 라이브러리 모음

##### 라이브러리 모음
```python
# 셀레니움 다운로드
%pip install selenium
# webdriver-manager 설치 
%pip install webdriver-manager

# 크롬 웹드라이버 구동 라이브러리
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

# 마우스,키보드 조작 라이브러리
from selenium.webdriver import ActionChains,Keys
from selenium.webdriver.common.by import By
from selenium.webdriver.common.actions.action_builder import ActionBuilder

# 비동기 처리 sleep으로 처리하기 위한 time 라이브러리
import time
```



## 마우스 이벤트 처리하기

### Mouse Event

- 웹 페이지에서 일어나는 일들을 Event라고 한다.

- 마우스로 일어날 수  있는 이벤트

  - 마우스 움직이기 (move)
  - 마우스 누르기 (press down)
  - 마우스 떼게 (press up)
  - ...

- 원하는 동작 : 마우스를 움직여서 목표 버튼을 누르는 것

  1. 입력하고자 하는 대상 요소를 찾는다. `find_element()`

  2. 입력하고자 하는 내용을 `click`을 통해 전달

  3. `.perform()`을 통해 동작. 이전까지의 쌓인 Action들을 실행

     - ```python
       # 예시
       
       # 입력하고자 하는 버튼을 찾아 button으로 저장
       button = driver.find_element(By.ID,"button")
       # ActionCahins : 동작을 이어주는것 (Ctrl을 누른 상태에서 C를 누르듯이) 지금은 클릭만
       ActionChains(driver).click(button).perform()
       ```

       

- ```python
  from selenium import webdriver
  from selenium.webdriver import ActionChains
  from selenium.webdriver.chrome.service import Service
  from selenium.webdriver.common.by import By
  from webdriver_manager.chrome import ChromeDriverManager
  
  driver = webdriver.Chrome(service = Service(ChromeDriverManager().install()))
  
  driver.get("https://www.naver.com")
  
  driver.implicitly_wait(0.5)
  
  button = driver.find_element(By.XPATH,'//*[@id="account"]/a')
  
  ActionChains(driver).click(button).perform()
  ```




## 키보드 이벤트 처리하기



### Keyboard Event

- 키보드로 일어날 수 있는 이벤트

  - 키보드 누르기 (press down)

  - 키보드 떼기 (press up)

  - ...

- 원하는 동작 : 마우스를 움직여서 목표 버튼을 누르는 것

  1. 입력하고자 하는 대상 요소를 찾는다. `find_element()`

  2. 입력하고자 하는 내용을 `send_key_to_element`을 통해 전달

  3. `.perform()`을 통해 동작. 

     - ```python
         # 예시
         
         # 입력하고자 하는 버튼을 찾아 button으로 저장
         text_input = driver.find_element(By.ID,"textInput")
         ActionChains(driver).send_key_to_element(textInput,"abc").perform()
       ```

- ```python
  from selenium import webdriver
  from selenium.webdriver import ActionChains,Keys
  from selenium.webdriver.chrome.service import Service
  from selenium.webdriver.common.by import By
  from webdriver_manager.chrome import ChromeDriverManager
  from selenium.webdriver.common.actions.action_builder import ActionBuilder
  
  import time
  
  # 네이버 창을 킨다
  driver = webdriver.Chrome(service = Service(ChromeDriverManager().install()))
  driver.get("https://www.naver.com")
  driver.implicitly_wait(1)
  
  # 로그인 버튼을 찾아 누른다.
  button = driver.find_element(By.XPATH,'//*[@id="account"]/a')
  ActionChains(driver).click(button).perform()
  
  # 아이디 input 요소를 찾아 아이디,비밀번호를 입력한다.
  id_input = driver.find_element(By.ID,"id")
  pw_input = driver.find_element(By.ID,"pw")
  ActionChains(driver).send_keys_to_element(id_input,"비밀!").perform()
  ActionChains(driver).send_keys_to_element(pw_input,"비밀!").perform()
  
  #  로그인 버튼을 찾아 누른다
  button = driver.find_element(By.XPATH,'//*[@id="log.login"]/span')
  ActionChains(driver).click(button).perform()
  ```
