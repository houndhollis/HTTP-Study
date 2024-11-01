## HTTP 헤더

용도

- HTTP 전송에 필요한 모든 부가정보
- 예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시관리정보
- 표준 헤더가 너무 많음
- 필요시 임의의 헤더 추가 가능

### 헤더 분류 RFC2616(과거)

- General 헤더: 메시지 전체에 적용되는 정보
- Request 헤더: 요청 정보 -> User-Agent: Mozilla/5.0
- Response 헤더: 응답 정보 -> Server: Apache
- Entity 헤더: 엔티티 바디 정보 -> Content-Type: text/html, Content-Length: 3423

### HTTP BODY RFC7230(최신)

HTTP/1.1 200OK

#### 표현 헤더 부분

Content-Type: text/html;charset=UTF-8
Content-Length: 3423

#### 표현 바디 부분

<html>
  <body>..</body>
</html>

- 메시지 본문 을 통해 표현 데이터 전달
- 메시지 본문 = 페이로드
- 표현은 요청이나 응답에서 전달할 실제 데이터
- 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공
  - 데이터 유형(html, json), 데이터 길이, 압축 정보 등등
- 참고: 표현 헤더는 표현 메타데이터와 페이로드 메시지를 구분해야 하지만

## 표현

- Content-Type : 표현 데이터의 형식
- Content-Encoding : 표현 데이터의 압축 방식
- Content-Language : 표현 데이터의 자연 언어
- Content-Length : 표현 데이터의 길이

- 표현 헤더는 전송, 응답 둘다 사용

### Content-Type

표현 데이터의 형식 설명

- 미디어 타입, 문자 인코딩
  - text/html; charset=utf-8
  - application/json
  - image/png

### Content-Encoding

표현 데이터 인코딩

- 표현 데이터를 압축하기 위해 사용
- 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
- 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
  예)
  - gzip
  - deflate
  - identity

### Content-Language

표현 데이터의 자연 언어

- 표현 데이터의 자연 언어를 표현
- 예)
  - ko
  - en
  - en-Us

### Content-Length

표현 데이터의 길이

- 바이트 단위

## 협상(콘텐츠 네고시에이션)

클라이언트가 선호하는 표현 요청

- Accept: 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset: 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
- Accept-Language: 클라이언트가 선호하는 자연 언어

> 협상 헤더는 요청시에만 사용

1. 만약 한국어 브라우저를 사용한다.
2. 외국에 있는 이벤트에 GET /event 를 했을 경우
3. 다중언어를 지원하지만 기본적으로 영어를 응답해준다.
4. Accept-Language: ko로 보내면 ko로 답을 해준다.

### 협상과 우선순위1

Quality Values(q) 값 사용

- 0~1, 클수록 높은 우선순위
- 생략하면 1
- Accept-Language: ko-KR,ko-q=0.9,en-US;q=0.8,en;q=0.7

### 협상과 우선순위2

구체적인 것이 우선한다.

- Accept: text/_, text/plain, text/plain;format=flowed, _/\*
  1. text/plain;format=flowed
  2. text/plain
  3. text/\*
  4. _/_

## 전송 방식 설명

- 단순 전송
  - 한번에 요쳥하고 한번에 받기
- 압축 전송
  - gzip 같은 경우 절반이상 줄어든다 크기가 , Content-Encoding 명시
- 분할 전송
  - Transfer-Encoding: chunked 쪼개서 보내며, Content-Length 를 보내면 안된다.
- 범위 전송
  - Content-Range: bytes 1001-2000 / 2000 범위를 지정해서 요청 할 수 있다.

## 일반 정보

### From

유저 에이전트의 이메일 정보

- 일반적으로 잘 사용되지 않음
- 검색 엔진 같은 곳에서, 주로 사용
- 요청에서 사용

### Referer

이전 웹 페이지 주소

- 현재 요청된 페이지의 이전 웹 페이지 주소

1. 구글에서 hello 검색
2. 다른 사이트 접속 후 referer 확인해보면, 구글이 명시되어 있다.

- Referer를 사용해서 유입 경로 분석 가능
- 요청에서 사용

### User-Agent

유저 에이전트 애플리케이션 정보

- 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
- 요청에서 사용

### Server

요청을 처리하는 ORIGIN 서버의 소프트웨어 정보

- Server: Apache/2.2.22 (Debian)
- server: nginx
- 응답에서 사용

### Date

메세지가 발생한 날짜와 시간

- 응답에서 사용

## 특별한 정보

### Host

요청한 호스트 정보 도메인

- 요청에서 사용
- 필수
- 하나의 서버가 여러 도메인을 처리해야 할 때
- 하나의 IP 주소에 여러 도메인이 적용되어 있을 때

### Location

페이지 리다이렉션

- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면 ,Location 위치로 자동 이동

### Allow

허용 가능한 HTTP 메서드

- 405 에서 응답에 포함해야함
- Allow: GET,HEAD,PUT
  - 위에 정보만 지원하기 때문에 다른 메서드를 쓰면 발생한다.

### Retry-After

유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간

- 503 서비스가 언제까지 불능인지 알려줄 수 있음

## 인증

### Authorization

클라이언트 인증 정보를 서버에 전달

- Authorization: Basic xxxxxxx
  인증에 문제가 있으면 401 Unauthorized 응답과 함께 사용

## 쿠키

- Set-Cookie: 서버에서 클라이언트로 쿠키 전달(응답)
- Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

### Stateless

- HTTP는 무상태 프로토콜이다
- 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊긴다.
- 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못한다.
- 클라이언트와 서버는 서로 상태를 유지하지 않는다.

### 쿠키 로그인

1. 웹브라우저가 로그인을 하면
2. 서버가 Set-Cookie: user = 홍길동 , 홍길동님이 로그인 했습니다.
3. 로그인 이후 페이지 접근 시 자동으로 쿠키값을 찾아서 요청에 넣어서 보낸다

- 예) set-cookie : sessionId=abcde1234; expires=Sat, 26-Dec-2020 00:00:00 GTM; path=/; domain=.google.com; Secure
- 사용처
  - 사용자 로그인 세션 관리
  - 광고 정보 트래킹
- 쿠키 정보는 항상 서버에 전송됨
  - 네트워크 트래픽 추가 유발
  - 최소한의 정보만 사용(세션 id, 인증 토큰)
  - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지
- 주의
  - 보안에 민감한 데이터는 저장하면 안됨

### 쿠키 - 생명주기

Expires, max-age

- Set-Cookie: expires=Sat, 26-Dec xxxx
  - 만료일이 되면 쿠키 삭제
- Set-Cookie: max-age=3600 (3600초)
  - 0이나 음수를 지정하면 쿠키 삭제
- 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
- 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지

### 쿠키 - 도메인

- 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함
  - domain=example.org를 지정해서 쿠키 생성
    - example.org는 물론
    - dev.example.org 도 쿠키 접근
- 현재 문서 기준 도메인만 적용
  - example.org 에서 쿠키를 생성하고 domain 지정을 생략
    - example.org 에서만 쿠키접근
    - dev.example.org는 쿠키 미접근

### 쿠키 - 경로

- 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
- 일반적으로 path=/ 루트로 지정
- 예)
  - path=/home 지정
  - /home/level1 가능

### 쿠키 - 보안

- Secure
  - 쿠키는 http, https를 구분하지 않고 전송
  - Secure를 적용하면 https인 경우에만 전송
- HttpOnly
  - XSS 공격 방지
  - 자바스크립트에서 접근 불가
  - HTTP 전송에만 사용
- SameSite
  - XSRF 공격방지
  - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송
