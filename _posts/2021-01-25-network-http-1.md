---
title: "HTTP의 이해"
categories:
  - categories/network
---

## HTTP(HiperText Transfer Protocol) 란? 
애플리케이션 레벨의 프로토콜 (OSI 7). 초월문서 전송 규약인 HTTP는 신뢰할만한 전송 혹은 세션 레이어의 **연결(Connection)**을 통해
메시지를 주고받는 상태가 없는 요청/응답 프로토콜이다.

**HTTP 클라이언트**는 서버와 연결을 맺고 HTTP 메시지를 보내는 프로그램  
**HTTP 서버**는 클라이언트의 연결을 수락하고 HTTP 요청을 처리하여 응답을 보내주는 프로그램

HTTP V1.1(1999) Original = RFC2616 이라고도 부름  
RFC 7230, 7231, 7230 시리즈 : 2616의 확장판  
HTTP V2.0(2015.5)

### Architecture
UA(user agent) : 사용자를 대신하여 일을 수행하는 소프트웨어 에이전트 
UA가 O(origin)에게 request를 보내고 origin이 response를 해주는 구조

### HTTP 메시지의 구조

| Start Line |
| Header |
| Blank Line |
| Body |

HTTP-message = Request | Response  
- Request  
Request-Line  
*(message-header CRLF(carriage return line feed))  
CRLF  
[ message-body ] - (optional choice)

- Response  
Status-Line  
*(message-header CRLF)  
CRLF  
[ messeag-body ]  

- Request Line  
구조 : method SP(space) request-target SP HTTP-version CRLF  
Ex) GET /hello.txt HTTP/1.1  

- Status(Response) Line  
구조 : HTTP-version SP status-code SP reason-phrase CRLF  
Ex) HTTP/1.1 404 Not Found

- Requset Message  
\<form action="/sign-up" method="POST"\>  
  \<input type="text" name="loginId"\>  
  \<input type="text" name="email"\>  
  \<input type="submit"\>  
\</form>  
POST /sign-up(URI) HTTP/1.1  
Host: example.com  
Connection: keep-alive  
Pragma: no-cache  
Cache-Control: no-cache  
loginId=doortts$email=sw.chae@naver.com  

- Response Message  
HTTP/1.1 200 OK  
Date: Sat, 19 Dec  
Cache-Control: no-cache  
Content-Type: text/html; charset=utf-8  
Vary: Accept  
Content-Length: 36278  
Connection: close  
\<!DOCTYPE html\>  
\<html\>  
...  

### Header Fields
Header-Name: header value
헤더 이름이나 콜론(:) 사이에 공백이 들어 가면 안된다.  
헤더 이름은 꼭 Capitalize(대문자로) 쓰지 않아도 된다. 

- Content Type  
  - primary object type / specific subtype  
  text/html, text/plain  
  image/jpeg, image/png  
  application/json, application/xml 등  
  - charset이 파라미터로 붙기도 한다  
  content-type: application/xml;charset=utf-8  

- HTTP Method  
GET, POST, PUT, DELETE, PATCH, OPTIONS, HEAD, TRACE, CONNECT  

| Method | Safe | Idempotent |  |
|:--:|:--:|:--:|:--:|
| GET |  |  |  |
| HEAD | o | o | get header only |
| PUT | x | o | 업데이트 |
| POST | x | x | 생성 |
| TRACE | o | o | echo back |
| OPTIONS | o | o | check server capagilities |
| DELETE | x | o | resource 삭제요청 |  

 safe : 근본적으로 read-only인 효과를 의도한 요청.  
 idempotent : 여러번 요청해도 의도한 효과는 한번 요청한 것과 같다.  

- GET  
서버의 리소스를 달라(read)  
Ex) GET /companies HTTP/1.1  
Host: example.org:8080

- POST  
서버에게 데이터를 보내겠다(create)  
Ex) POST /companies HTTP/1.1  
Host: example.org:8080  
Content-Type: application/json  
{"name": "naver", "location": "seoul"}

- PUT  
서버가 갖고 있는 리소스를 내가 보내는 것으로 대체해라(create or replace)  
Ex) PUT /companies/1 HTTP/1.1  
Host: example.org:8080  
Content-Type: application/json  
{"name": "naver", "location": "seoul"}
  - 주의: 리소스를 "수정"하는 요청이 아님


- DELETE  
서버가 갖고 있는 리소스를 삭제하라  
Ex) DELETE /companies/1 HTTP/1.1  
Host: example.org:8080  

- PATCH  
서버가 갖고 있는 리소스를 수정하라(update)  
Ex) PATCH /companies/1 HTTP/1.1  
Host: example.org:8080  
Content-Type: application/json-patch+json  
If-Match: "abc123"  
[{"op": "replace", "path": "/location", "value": "seoul"}]  

- OPTIONS  
특정 URL에 대한 옵션들을 알려달라 (보통 사용 가능한 메소드를 반환)  

- HEAD  
GET으로 요청하면 어떤 헤더들을 받게 될지 알려달라 (GET과 같으나 본문이 없음)

- TRACE  
내 요청이 서버에 갔다가 응답이 돌아올 때 까지 어떤 프록시들을 거치는지 알려달라

- CONNECT  
SSL 터널링 등 특수 용도에 사용하는 메소드

### HTTP 상태 코드(Status Code)
- 1xx - informational (100, 101)
  - 100 Continue : 계속 해도 좋다
  - 101 Switching Protocols : 다른 프로토콜을 사용한다
- 2xx - Successful (200 ~ 206) : 성공했다
  - 200 OK :  
  - 201 Created : 
  - 202 Accepted : 
- 3xx - Redirection (300 ~ 307) : 다른 데를 알아봐라
  - 301 Moved Permanently : 
  - 302 Moved temporarily (Found) : 
  - 303 See other : 
  - 304 Not Modified : 
  - 307 Temporary Redirect : 
  - 308 Permanen Redirect : 
- 4xx - Client error (400 ~ 417) :  클라이언트가 잘못했다
  - 400 : Bad Request
  - 401 : Unauthorized
  - 403 : Forbidden
  - 404 : Not Found
- 5xx - Server error (500 ~ 505) :  서버가 잘못했다
 - 500 : Internal Server error


### HTTP 트랜잭션 과정
웹 브라우저는 열린 TCP Connection에 "GET /index.html HTTP/1.1" 요청을 보낸다.  

GET /index.html HTTP/1.1  
Host: www.example.org  

서버는 TCP Connection을 통해 들어온 "GET /index.html HTTP/1.1" 를 읽고 /index.html을 요청함을 확인한다.  
서버는 /index.html의 내용을 본문으로 하는 HTTP 응답메시지를 만들어 이를 클라이언트에게 보내주기 위해 TCP Connection에 쓴다. 

HTTP/1.1 200 OK  
Content-Length: 83  
Content-Type: text.html;charset=utf-8  
Date: Wed, 14 Feb 2013  

\<html\>  
\<head\>\</head\>  
\<body\>Hello, World\</body\>  
\</html\>  

웹 브라우저는 TCP Connection을 통해 들어온 HTTP 응답 메시지를 읽고, 시작줄과 헤더를 파싱해서 성공적인 응답이 돌아왔음을 확인한다.  

- URI (Uniform Resource Identifier) >= URL (Uniform Resource Locator), URN (Uniform Resoruce Name)


### Conditional GET
클라이언트는 이전에 한번 요청해서 돌려받은 리소스에 대해 다시 한번 요청을 할 때,
불필요한 트래픽을 줄이기 위해 해당 리소스가 변경된 경우에만 다시 보내달라고 요청할 수 있다.

- Conditional GET을 위해 서버에서 클라이언트로 전송하는 정보  
  - Last-Modified:HTTP-date  
  Last-Modified는 Date보다 이후여서는 안된다.  
  서버는 가능한한 항상 Last-Modified 헤더를 보내야한다. 

    - response message -> GET /index.html HTTP/1.1  
    - request message -> HTTP/1.1 200 OK  
    Last-Modified: Sat, 2 Feb 2013...  
    - re-response message -> GET...  
    If-Modified-Since: Sate, 2 Feb 2013...  
    - reqeust message -> HTTP.1.1 304 Not Modified  
    none body  

  - ETag:entity-tag  
  리소스가 바뀌면 바뀌어야 한다.

    - response message -> GET /index.html HTTP/1.1  
    - request message -> HTTP/1.1 200 OK  
    ETag: "6af305a""
    - re-response message -> GET...  
    If-None-Match ETag: "6af305a""
    - reqeust message -> HTTP.1.1 304 Not Modified  
    nono body  


### Cache
캐시는 클라이언트와 서바 사이를 오가는 HTTP 메시지를 저장한 후 재활용하여,
요청의 반응시간과 네트워크의 트래픽을 모두 줄여준다.  
Conditional Get을 이용한 트래픽을 줄일 수는 있지만, Round Trip은 줄일 수 없기 때문에 사용.  

가장 많은 리소스를 가진 html은 no-cash 한다.  

- 캐시의 기본 동작  
  - 몇가지 예외를 제외하면, 캐시는 성공한 모든 응답을 저장한다.  
  - 캐시는 fresh한 entity의 경우, validation 없이 반환할 수 있다.
  - 캐시는 stale한 entity의 경우, validation에 성공했다면 반환한다.  
  - 위와 같은 기본 동작을 수정하고자 한다면 Cache-Control 헤더를 사용하면 된다.  


### State Management
http는 state-less이다. 로그인이나 장바구니 카트에 넣은 목록을 기억하게 하고 싶다. 따라서 URL에 상태 정보를 몽땅 집어넣는
Fat URL방법이나, 클라이언트 IP를 보고 누가 어떤 상태인지 서버가 기억하는 IP추적 방법, HTTP의 인증 메커니즘을 사용하는
Authentication 방법등이 있다.  
이러한 방법중에서 가장 많이 쓰이는 방법은 Cookie 방법이다.  


### Cookie
Set-Cookie 응답헤더를 통해 쿠키를 서버에서 클라이언트로 보내면, 클라이언트는 쿠키를 보관하고 있다가
필요시 Cookie 헤더를 통해 서버에 보내준다.  

- Response Header  
  - Set-Cookie: NAME=VALUE; expires=DATE; path=PATH; domain=DOMAIN_NAME; secure
- Request Header
  - Cookie: NAME1=OPAQUE_STRING1; NAME2=OPAQUE_STRING2 ...  

Session cookie는 브라우저가 닫으면 사라지고, Persistent cookie는 도메인 지정여부에 따라 동작이 다르다.  


### HTTPS와 인증
HTTP Over TLS(Transport Layer Security)로 TLS 위에서 동작하는 HTTP로 HTTP 메시지를 암호화하여 주고받는다.  
서버가 클라이언트에게 인증서를 보내어 자신을 증명한다.  
클라이언트는 인증서를 검증하고 이상이 없으면 HTTPS 커넥션을 시작한다.  

개인기 공개키를 이용해서 서로 인증 가능한 비밀키를 공유하는 방법으로 인증한다.  

### HTTP/2
HTTP/1.1 보다 빠르다. 

- 기능
  - Header Compression: 헤더도 압축한다.
  - Multiplexed Streams: Connection을 한번 맺고 여러 개의 파일을 동시에 주고 받을 수 있다.
  - Server Push: html을 요청하면 css나 javascript도 동시에 줄 수 있다.
  - Stream Priority: 리소스간에 우선순위를 부여해서 전송할 수 있다.
  

### HTTP/3
HTTP/2 over QUIC(Quick UDP) 기반. 모바일에서 네트워크가 계속 끊끼고 손실이 일어날 상황을 가정해서 만들었다.





















