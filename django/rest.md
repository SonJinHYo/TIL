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


## serializer
1. serializer는 단순한 번역기이다
2. 사용자가 우리 서버로 데이터를 보낼수 있게 하면서 request.data를 통해 데이터를 보낼수 있다
3. 우리는 사용자를 신뢰하지 못하기 때문에
is_valid
serializer.save()는 serializer의 create메소드를 부를것이다

update같은 경우는 사용자가 수정하고 싶어하는 category의 데이터와 사용자가 보낸 데이터로 만들것이다
기본값 -> instance.name
