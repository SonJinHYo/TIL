Domain name 
참조 : https://www.cloudflare.com/en-gb/learning/dns/glossary/what-is-a-domain-name/

## Domain name 

알파벳과 숫자로 된 IP 주소와 매핑되는 텍스트 문자열로, 클라이언트 소프트웨어를 통해 웹 사이트에 접근하는 데 사용

- 도메인 이름은 사용자가 특정 웹 사이트에 도달하기 위해 브라우저 창에 입력하는 텍스트이다. 

실제 웹 사이트의 주소는 복잡한 숫자로 된 IP 주소(예: 192.0.2.2)이지만, DNS 덕분에 사용자는 사람 친화적인 도메인 이름을 입력하고 원하는 웹 사이트로 라우팅될 수 있습니다. 이 과정을 DNS 조회라고 합니다.

## 도메인 이름 관리 

도메인 이름은 도메인 레지스트리에 의해 모두 관리되며 도메인 이름의 예약을 등록기에 위임한다. 웹 사이트를 생성하고자 하는 누구나 등록기와 도메인 이름을 등록할 수 있으며 현재 등록된 도메인 이름은 3억 개가 넘는다.

## 도메인 이름과 URL의 차이점 

URL(또는 웹 주소)은 **사이트의 도메인 이름과 프로토콜, 경로를 포함한 기타 정보를 포함**합니다. 

- 예를 들어, URL 'https://cloudflare.com/learning/'에서 'cloudflare.com'은 도메인 이름이고, 'https'는 프로토콜이고, '/learning/'은 웹 사이트의 특정 페이지로의 경로입니다.

## 도메인 이름의 구성 요소

도메인 이름은 일반적으로 점으로 구분되는 두 개 또는 세 개의 부분으로 나누어진다. 

도메인 이름의 식별자는 오른쪽에서 왼쪽으로 읽을 때 가장 일반적인 것부터 가장 구체적인 것까지 차례대로 나열된다. 도메인 이름에서 가장 오른쪽의 마지막 점 오른쪽 부분이 최상위 도메인(TLD:Top Level Domain)입니다.

- TLD는 '.com', '.net', '.org'와 같은 '일반' TLD뿐만 아니라 '.uk', '.jp'와 같은 국가별 TLD도 포함한다.



TLD의 왼쪽에는 2차 도메인(2LD)이 있고, 만약 2LD의 왼쪽에 무언가 있다면, 이를 3차 도메인(3LD)이라고 합니다. 몇 가지 예시를 살펴보겠습니다:

Google의 미국 도메인 이름인 'google.com'의 경우:

- '.com'은 TLD  /'google'은 2LD 

Google의 영국 도메인 이름인 'google.co.uk'의 경우:

- '.com'은 TLD /'.co'는 2LD /'google'은 3LD 
  - 이 경우 2LD는 도메인을 등록한 조직의 유형을 나타냅니다 (영국에서의 '.co'는 회사에 의해 등록된 사이트를 나타낸다).

## 도메인 이름을 보호하는 방법 

1. 한번 도메인 이름이 등록된 후
2. 등록 대행업체가 도메인이 만료되기 전에 등록자에게 통지하고
3. 도메인 이름을 갱신할 기회를 제공하여 도메인을 잃지 않도록 관리한다. 

일부 경우에는 등록 대행업체가 도메인이 만료되는 즉시 해당 도메인을 사들여 원래의 등록자에게 과도한 가격으로 다시 판매하는 등의  악의적인 수법을 사용할 수도 있다. 이 때문에 신뢰성과 정직성이 있는 등록 대행업체를 선택하는 것이 중요하다.