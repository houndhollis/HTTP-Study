## HTTP(HyperText Transfer Protocol)

### 모든 것이 HTTP

HTTP 메세지에 모든것을 전송한다.

- HTML, TEXT
- Image, 음성, 영상, 파일
- JSON, XML (API)
- 거의 모든 형태의 데이터 전송 가능
- 서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용
- 지금은 HTTP 시대!

### HTTP 역사

HTTP/1.1 - 1997년 ; 가장 많이 사용, 우리에게 가장 중요한 버전
HTTP/2 2015년 : 성능개선
HTTP/3 진행중 : TCP 대신에 UDP 사용, 성능 개선

### 기반 프로토콜

TCP: HTTP/1.1, HTTP/2
UDP : HTTP/3

- 현재 HTTP/1.1 주로 사용
  - HTTP/2, HTTP/3 도 점점 증가

### HTTP 특징

- 클라이언트 서버 구조
- 무상태 프로토콜(stateless), 비연결성
- HTTP 메세지
- 단순함, 확장 가능

## 클라이언트 서버 구조

- Request Response 구조
- 클라이언트는 서버에 요청을 보내고, 응답을 대기
- 서버가 요청에 대한 결과를 만들어서 응답

## 무상태 프로토콜

스테이트리스(Stateless)

- 서버가 클라이언트의 상태를 보존 X
- 장점 : 서버 확장성 높음(스케일 아웃)
- 단점 : 클라이언트가 추가 데이터 전송

### Stateful, Stateless 차이

상태 유지 - Stateful

Stateful 의 경우 서버가 클라이언트의 이전 상태를 보존을 하지만, Stateless 같은 경우는 보존하지 않는다.

- 상태 유지 : 중간에 바뀌면 상태를 다음 연결 점에 미리 알려줘야 한다.
- 무상턔 : 중간에 바뀌어도 상관이 없다, 무상태는 응답 서버를 쉽게 바꿀 수 있다 -> 무한한 서버 증설 가능, 갑자기 클라이언트 요청이 증가해도 서버를 대거 투입할 수 있다.

> 즉, 상태 유지 같은 경우는 항상 같은 서버가 유지되어야 한다. 클라이언트 A - 서버 1 계속 이어져서 응답이 이루어져야 한다. 만약 서버1번이 죽어버리면, 클라이언트 A 는 처음부터 다시 시작해야 한다. 하지만 무상태 경우는, 클라이언트가 데이터를 다 담아서 보내기 때문에 이러한 문제를 해결할 수 있다.

- 스케일 아웃 - 수평 확장 유리

### Stateless

실무 한계

- 모든 것을 무상태로 설계 할 수 있는 경우도 있고 없는 경우도 있다.
- 무상태
  - 예) 로그인이 필요 없는 단순한 서비스 소개 화면
- 상태 유지
  - 예) 로그인
- 로그인한 사용자의 경우 로그인 했다는 상태를 서버에 유지
- 일반적으로 브라우져 쿠키와 서버 세션등을 사용해서 상태 유지
- 상태 유지는 최소한만 사용

## 비 연결성

- HTTP는 기본이 연결을 유지하지 않는 모델
- 일반적으로 초 단위의 이하의 빠른 속도로 응답
- 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음
  - 예) 웹 브라우저에서 계속 연속해서 검색 버튼을 누르지는 않는다.
- 서버 자원을 매우 효율적으로 사용할 수 있음.

### 비 연결성

한계와 극복

- TCPI/IP 연결을 새로 맺어야 함 - 3 way handshake 시간 추가
- 웹 브라우저로 사이트를 요청하면 HTML 뿐만 아니라 자바스크립트, css 등 수 많은 자원이 함께 다운로드
- 지금은 HTTP 지속 연결(Persistent Connections)로 문제 해결
- HTTP/2, HTTP/3 에서 더 많은 최적화

## HTTP 메세지

모든 바이너리 데이터들을 다 보낼 수 있다.
HTTP 구조
start-line : 시작 라인
header : 헤더
empty line : 공백 라인(CRLF) 무조건 있어야 한다.
message body

### 시작 라인

#### 요청 메세지

HTTP 메서드
종류: GET, POST, PUT, DELETE
서버가 수행해야 할 동작 지정
<br/>
요청 메세지 - 요청 대상
절대경로 = '/' 로 시작하는 경로
<br/>
요청 메세지 - HTTP 버전
HTTP Version

#### 응답 메세지

start-line = request-line / status-line
status-line = HTTP-version SP status-code SP reason-phrase CRLF

- HTTP 상태 코드: 요청 성공, 실패 유무
  - 200 성공
  - 400 클라이언트 요청 오류
  - 500 서버 내부 오류

### HTTP 헤더

field-name":" OWS filed-value OWS (OWS: 띄어쓰기 허용), field-name 은 대소문자 구문 없음

#### HTTP 헤더 용도

- HTTP 전소엥 필요한 모든 부가정보
- 예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 애플리케이션 정보, 캐시 관리 정보
- 표준 헤더가 너무 많음
- 필요시 임의의 헤더 추가 가능
  - helloworld: hihi

### HTTP 메시지 바디

- 실제 전송할 데이터
- HTML 문서, 이미지, 영상, JSON 등등
