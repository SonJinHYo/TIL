# Selenium 기초(2) - Wait*Call



## Wait & Call

- Selenium은 동적 웹사이트에 대한 지원을 위해 명시적 기다림(Explicit Wait)과 암묵적 기다림 (Implicit Wait)을 지원한다.
  - Implicit Wait : 다 로딩이이 될 떄 까지 지정한 시간동안 기다림
    - eg. 로딩이 다 되기까지 5초간 기다려라
  - Explicit Wait : 특정 요소에 대한 제약을 통한 기다림
    - eg. 이 태그를 가져올 수 있을 때 까지 기다려라



### XPath

- **XML, HTML 문서 등의 요소의 위치를 경로로 표현하는 것**

- 태그의 id나 클래스이름으로는 스크래핑 하기가 힘든 경우에 사용

  - 무분별한 스크래핑을 막기 위해 클래스 이름을 랜덤하게 생성하기 때문
  - **위치**를 이용해서 스크래핑
    - eg. Desktop/폴더1/폴더2/파일.txt 같은 표현

- - 어느 사이트에 있는 한 태그를 <우클릭->copy->XPath> 를 해서 복사해온 것
  - @id="__next" : id 다음의 div 다음의 main 다음의... span 다음의 img !

  

 ```python
 from selenium import webdriver
 from selenium.webdriver.chrome.sevice import Service
 from selenium.webdriver.common.by import By
 from webdriver_manager.chrome import ChromeDriverManager

# 드라이버를 다운받고
driver = webdriver.Chrome(service = Service(ChromeDriverManager().install()))
# 한 사이트에 들어가서
driver.get("https://indistreet.com/live?sortOption=startDate%3ADESC")
# XPATH를 통해 element를 불러온다
driver.find_element(By.XPATH,'//*[@id="__next"]/div/main/div[2]/div/div[4]/div[1]/div[1]/div/a/div[1]/div/span/img')

>>>사이트는 뜨지만 요소가 없다는 요청을 받게된다!
 ```

- 요소가 없는 이유는?
  - 비동기 처리때문!



### Implicit Wait

- `.implicitly_wait(n)`
  - n초를 기다린다. **단, n초 후 실행이 아닌 최대 n초 까지 기다린다는 뜻**

```python
with webdriver.Chrome(service = Service(ChromeDriverManager().install())) as driver:
    driver.get("https://indistreet.com/live?sortOption=startDate%3ADESC")
    # 최대 10초 멈춰!
    driver.implicitly_wait(10)
    
    print(driver.find_element(By.XPATH,'//*[@id="__next"]/div/main/div[2]/div/div[4]/div[1]/div[1]/div/a/div[1]/div/span/img'))
    
""" 출력이 된다!

<selenium.webdriver.remote.webelement.WebElement (session="630e9ce4c0789c20aee301af41c309ca", element="ea4cc778-f282-4f61-8b6f-20db7b0ce446")>

"""
```



### Explicit Wait

- `WebDriverWait()`과 두 메서드를 활용하여 적용할 수 있다
  - `until()` : 인자의 조건이 만족될 때 까지
  - `until_not()` : 인자의 조건이 만족되지 않을 때 까지
- **eg** :  `element = WebDriverWait(driver,10).until(EC.presence_of_element_located((By.ID,"target")))`
  - element는, driver를 최대 10초까지 기다려라. 만족할 때 까지, 존재한다면, ID의 target이
    - EC : expected_conditions의 약자, selenium에서 정의한 조건

```python
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.ui import WebDriverWait

with webdriver.Chrome(service = Service(ChromeDriverManager().install())) as driver:
    driver.get("https://indistreet.com/live?sortOption=startDate%3ADESC")
    #암묵적 기다림
    element = WebDriverWait(driver,10).until(EC.presence_of_element_located((By.XPATH,'//*[@id="__next"]/div/main/div[2]/div/div[4]/div[1]/div[1]/div/a/div[1]/div/span/img')))
    
    print(element)
    
""" 같은 출력 확인 가능!

<selenium.webdriver.remote.webelement.WebElement (session="630e9ce4c0789c20aee301af41c309ca", element="ea4cc778-f282-4f61-8b6f-20db7b0ce446")>

"""
```





### 여러 자료를 가져오려면?

- 하나씩 긁어와봤더니..

  - //*[@id="__next"]/div/main/div[2]/div/div[4]/div[1]/div[1]/div/a/div[1]/div/span/img
  - //*[@id="__next"]/div/main/div[2]/div/div[4]/div[1]/div[2]/div/a/div[1]/div/span/img
  - //*[@id="__next"]/div/main/div[2]/div/div[4]/div[1]/div[3]/div/a/div[1]/div/span/img
  - 등등..

- 중간의 div만 바뀌는 것을 확인

  **=> for 문 활용**

```python
with webdriver.Chrome(service = Service(ChromeDriverManager().install())) as driver:
    driver.get("https://indistreet.com/live?sortOption=startDate%3ADESC")
    # 최대 10초 멈춰!
    driver.implicitly_wait(10)
    
    # 10개 가져오기
    for i in range(1,11):
        element = driver.find_element(By.XPATH,f'//*[@id="__next"]/div/main/div[2]/div/div[4]/div[1]/div[{i}]/div/a/div[1]/div/span/img')
        
```

