# 크롬 자동화 방지 우회

스크래핑을 배우고나서 쿠팡에 있는 구매목록들에 접근해서 지출내역 정리를 커스텀으로 자동화해보려 했으나 잘 안되었다.

쿠팡뿐만 아니라 어지간한 사이트는 스크래핑이 막혀있어 아래 옵션 설정을 통해 들어갈 수 있게되었다.

쿠팡은 이 방법도 안먹혀서 아예 다른 방법으로 접근해야 했다.. 우선 저장해두고 어디다 써먹을지 생각해 봐야겠다.

```python
# 크롬 웹드라이버 구동 라이브러리
## install selenium, webdriver_manager, fake_user

from tkinter import Button
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

# 마우스,키보드 조작 라이브러리
from selenium.webdriver import ActionChains,Keys
from selenium.webdriver.common.by import By
from selenium.webdriver.common.actions.action_builder import ActionBuilder

# fake-useragent 패키지 필요
from fake_useragent import UserAgent

# 비동기 처리 sleep으로 처리하기 위한 time 라이브러리
import time

# options : 크롬의 옵션을 저장
options = webdriver.ChromeOptions()

# 자동화 감지 피하기 옵션
options.add_argument("--disable-blink-features=AutomationControlled")

# 봇감지 피하기를 위한 임의의 에이전트 지정
user_ag = UserAgent().random
options.add_argument('user-agent=%s'%user_ag)

# 프로그램 자동화 안내메세지 삭제
options.add_experimental_option("excludeSwitches", ["enable-automation"])
options.add_experimental_option("useAutomationExtension", False)

# 이미지 받지않기 : 속도 빨라짐
options.add_argument('--blink-settings=imagesEnabled=false')

# 창 띄우기
driver = webdriver.Chrome('/mnt/c/chromedriver.exe',service=Service(ChromeDriverManager().install()),chrome_options=options)

# 크롤링 방지 undefined로 성정
driver.execute_cdp_cmd("Page.addScriptToEvaluateOnNewDocument", {
"source": """
        Object.defineProperty(navigator, 'webdriver', {
            get: () => undefined
        })
        """
})

# 이후 창 띄우기 (예시 프로그래머스 로그인 url)
url = 'https://programmers.co.kr/account/sign_in?referer=https%3A%2F%2Fprogrammers.co.kr%2F'
driver.get(url)
.
.
.
```
