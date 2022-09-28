# Selenium 배경,기초(1)



## 동적 웹사이트

- html이 계속해서 변하는 웹 사이트 : 새로고침 할 때 마다 새로운 내용이 올라오는 사이트
  - 인스타그램, 유튜브 등등..
- 정적 웹 사이트는 html 문서가 완전하게 응답한다.
- 동적 웹 사이트는 응답 후 html이 렌더링이 될 때 까지 지연시간이 존재한다.



## 동적 웹 사이트의 동작 방식

- 비동기 처리를 통해 필요한 데이터를 채운다.

  - 동기 처리 : 요청에 따른 응답을 기다린다. (렌더링이 끝나면 데이터를 처리한다)

  - 비동기 처리 : 요청에 따른 응답을 기다리지 않는다.(렌더링이 끝나지 않아도 데이터를 처리한다.)

    즉, 상황에 따라 받은 데이터가 완전하지 않은 경우가 발생할 수 있음.

    

- 단순한 웹 스크랩의 문제점 & 해결책

  1. 요청을 보내면 불완전한 응답을 받을 수 있다.
     - 해결책 : 임의의 시간을 지연한 후, 데이터 처리가 끝난 후 정보를 가져온다.
  2. 키보드 입력, 마우스 클링 등의 상호작용을 requests로 진행하기 어렵다. (요청만 하고 응답만 받기 때문)
     - 해결책 : 키보드 입력, 마우스 클릭 등을 프로그래밍으로 처리한다.

  **즉, 파이썬으로 웹 브라우저를 조작하자**



## Selenium

- Python을 이용해서 웹 브라우저를 조작할 수 있는 자동화 프레임워크

- 셀레니움을 다운받고, 웹브라우저를 연동해주는 'WebDriver'가 필요하므로 같이 다운로드

- `WebDriver`은 웹브라우저를 제어할 수 있는 프레임워크 (지금은 크롬 기준으로)

- ```python
  # 셀레니움 다운로드
  %pip install selenium
  # webdriver-manager 설치 
  %pip install webdriver-manager
  ```



### Selenium 시작하기

```python
from selenium import webdriver
# 크롬 객체를 만들 때 인자로 넣어줄 예정
from selenium.webdriver.chrome.service import Service
# 사용하고 있는 크롬과 싱크하기 위해
from webdriver_manager.chrome import ChromeDriverManager


# 내가 사용중인 크롬에 맞춰서 크롬드라이버 객체를 만들어 driver에 저장
driver = webdriver.Chrome(service = Service(ChromeDriverManager().install()))
>>> driver 실행 시 빈 크롬창이 뜬다
```



실행 후 다시 창을 닫는 코드를 써서 driver을 종료시켜 주어야 한다.

(직접 꺼도 되지만 자동화니까)

**with-as 구문으로 명령**

```python
# with (이것을 실행해서) as (이거라 하자)
with webdriver.Chrome(service = Service(ChromeDriverManager().install())) as driver:
    driver.get("https://www.example.com")
    print(driver.page_source)

""" 페이지 소스 출력을 확인 가능

<html><head>
    <title>Example Domain</title>

    <meta charset="utf-8">
    <meta http-equiv="Content-type" content="text/html; charset=utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style type="text/css">
    body {
        background-color: #f0f0f2;
        margin: 0;
        padding: 0;
        font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;
        
    }
    div {
        width: 600px;
        margin: 5em auto;
        padding: 2em;
        background-color: #fdfdff;
        border-radius: 0.5em;
        box-shadow: 2px 3px 7px 2px rgba(0,0,0,0.02);
    }
    a:link, a:visited {
        color: #38488f;
        text-decoration: none;
    }
    @media (max-width: 700px) {
        div {
            margin: 0 auto;
            width: auto;
        }
    }
    </style>    
</head>

<body>
<div>
    <h1>Example Domain</h1>
    <p>This domain is for use in illustrative examples in documents. You may use this
    domain in literature without prior coordination or asking for permission.</p>
    <p><a href="https://www.iana.org/domains/example">More information...</a></p>
</div>


</body></html>

"""
```



### 특정 요소 추출하기

- 요소 하나 찾기 - find_element ( by, target )
  - by : 대상을 찾는 기준 : ID , TAG_NAME, CLASS_NAME, ...
  - target : 대상의 속성

- 요소 여러개 찾기 - find_elements( by , target )

  - by : 대상을 찾는 기준 : ID , TAG_NAME, CLASS_NAME, ...

  - target : 대상의 속성
  - 리스트로 반환한다!

```python
# By를 import 해주기
from selenium.webdriver.common.by import By

# p태그의 요소 찾기 
with webdriver.Chrome(service = Service(ChromeDriverManager().install())) as driver:
    driver.get("https://www.example.com")
	print(driver.find_element(By.TAG_NAME,"p").text)
    
""" p태그의 text가 출력된다.

This domain is for use in illustrative examples in documents. You may use this domain in literature without prior coordination or asking for permission.

"""


# p태그의 요소 여러개 찾기 
with webdriver.Chrome(service = Service(ChromeDriverManager().install())) as driver:
    driver.get("https://www.example.com")
    # elements는 리스트로 반환하기 때문에 for문으로 출력해본다.
    for element in driver.find_elements(By.TAG_NAME,"p"):
        print("Text:",element.text)
        
""" 2개 출력 확인

Text: This domain is for use in illustrative examples in documents. You may use this domain in literature without prior coordination or asking for permission.
Text: More information...

"""
```

