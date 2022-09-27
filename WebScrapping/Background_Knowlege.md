# 웹스크래핑 기초지식

컴퓨터의 진화 과정

계산기(computer)
.
.
컴퓨터 2개 연결 (NetWork)
.
.
씨족사회부터 국가까지. (Local Area Network, LAN)
.
.
지구연합  (Inter Network, InterNet)

.

.

.

이렇게 빨리 지구연합이 될 수 있는 이유는?



World Wide Web, www, Web 이라고 부르는 환경 속에서 자기들끼리 정보를 빠르게 주고받을 수 있었던 것이다.



어떻게 주고받는가?



1. Request : 저한테 필요한 정보가 있을까요?

   - 요청하는 쪽을 'Client'라 한다. (Client의 request를 받는다)

   - www.naver.com 주세용
   - www.google.com 주세용

2. response : 네 여기 있습니당

   - 제공하는 쪽을 'Server'라 한다. (Server의 response를 받았다.)
   - 으악 서버 터졌다! (요청이 폭주해서 처리가 안된다!)
   - ?? 이상한걸 요청하셨네용? (잘못된 요청에 대한 반응도 내놓는다)



이런 대화가 이루어지려면 통일된 형식이 있어야한다.

**그것이 HTTP**



## HTTP

- Hypertext Transfer Protocol
- HTTP 요청 : Client의 Request
- HTTP 응답 : Server의 Response



### HTTP의 정보

- Head : request/response 의 정보를 담는다

  - Host : 누가 받는가(보내는가)

  - Resorece : 어디로 주면(받으면) 되는가

  - Method : 어떻게 주면(받으면) 되는가
  - 등등..
- Body : 내용물
  - (뭐가 많음)







## 웹사이트, 웹페이지



### 웹 사이트

- 웹 페이지들의 모임



### 웹 페이지

- 홈페이지 등등 사이트 접속 시 보이는 여러 화면 중 하나
- response의 응답으로 받는 것
- HTTP응답에 담긴 HTML문서를 '웹브라우저'가 그려서 보여주는것



## HTML

- HyperText Markup Language

- head와 body로 나뉘어진다.
- head : 문서에 대한 정보 - 언어, 제목, 등 문서의 정보
- body : 우리에게 보여지는 주 내용. 문서의 내용(글, 영상 등)



### 웹브라우저

웹브라우저는 html을 DOM으로 받아들이고 읽는다.



#### DOM (Doucument Object Model)

- 브라우저에 대한 넓고 얕은 지식

- 구조 (단순하진 않음)

  Document

  - html

    - head

      - style
        - attribute:type

    - body

      - div

        - attribute:type

      - ul

        - h2

        - li

          - attribute:type

          .

          .

        - li

          - attribute:type

      - script

        - attribute:type

      - ul

        - attribute:type

- 이런 각 노드들을 객체로 생각하면 문서를 더욱 편리하게 관리 가능

- 브라우저는 '랜더링'을 통해 DOM을 작성하고, 이런 DOM Tree를 순회해서 특정 원소를 찾거나, 추가, 삭제 할 수 있음

  

- 요약 : 브라우저는 html을 dom으로 바꿈

  - 원하는 요소를 동적으로 바꿔주거나 ,**찾거나** 등 할 수 있기 때문. (Scrapping에 유용)






