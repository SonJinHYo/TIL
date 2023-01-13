# REST API
1.  django REST API Framework 다운로드 : djangorestframework install
  - poetry : `poetry add djangorestframework`
  - python terminal : `pip install djangorestframework`
2. config.setting의 INSTALLED_APPS 에 `"rest_framework"` 추가


## REST API는
- 동사를 가지지 않는다
- HTTP(Hypertext Transfer Protocol)를 통해 url 이동없이 정보를 주고받는다.
  - HTTP 사용 = 링크 기반으로 데이터에 접근
- 어떤 종류의 데이터든 전송할 수 있다.
- Method : 기본적으로 GET, POST, PUT, DELETE가 있다.
  - 참고 : https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods


