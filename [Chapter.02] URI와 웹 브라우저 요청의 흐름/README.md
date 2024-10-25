## URI (Uniform Resource Identifier)

URI는 로케이터(locator), 이름(nmae) 또는 둘 다 추가로 분류될 수 있다

URI는 URL 과 URN을 포함한다.

### URL 단어 뜻

Uniform: 리소스를 식별하는 통일된 방식

Resource: 자원, URL로 식별할 수 있는 모든 것(제한 없음)

Identifier: 다른 항목과 구분하는데 필요한 정보

### URL, URN

URL - Locator : 리소스가 있는 위치를 지정

URN - Name : 리소스에 이름을 부여

> 위치는 변할 수 있지만, 이름은 변하지 않는다.

URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음,
앞으로 URI 를 URL과 같은 의미로 이야기 한다.

### URL 전체 문법

- Scheme://[userinfo@]host[:port][/path][?query][#fragment]

- https://www.google.com:443/search?q=hello&hl=ko

- 프로토콜 (https)
  - 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙 (http, https, ftp)
- 호스트명 (www.google.com)
  - 도메인 명이나 아이피 주소 사용 가능
- 포트 번호 (443) -> 일반적인 포트는 생략 가능
- 패스 (/search)
- 쿼리 파라미터 (q=hello&hl=ko)

> userinfo@ 는 URL에 사용자 정보를 포함해서 인증할 경우, 거의 사용하지 않음
> fragment html 내부 북마크 등에 사용

## 웹 브라우저 요청 흐름

1. https://www.google.com:443/search?q=hello&hl=ko 를 입력
2. DNS 서버를 조회하여 IP주소를 찾아낸다.
3. HTTP 요청 메세지를 생성한다.
4. Socket 라이브러리를 통해 전달된 후
5. TCP/IP 연결을 한다. (IP, PORT), 데이터 전달
6. TCP/IP 패킷 생성, HTTP 메세지 포함
7. 인터넷상 노드들을 통해 목적지 도착
8. 요청 패킷이 도착하면 TCP/IP 패킷을 버리고 HTTP 메세지를 본 후 응답을 준다
9. 응답을 받으면 그 후 렌더링 시켜 사용자에게 보여준다.
