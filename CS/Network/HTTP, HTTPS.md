# HTTP(Hyper Text Transfer Protocol)
> 웹 서버와 클라이언트 간의 데이터를 주고받기 위한 통신 규약입니다.

## 특징
- Application Layer 프로토콜, :80
- 무상태(Stateless) 프로토콜 상태를 유지하기 위해 `Cookie`와 `Session` 사용

## HTTP 요청 메서드
1. GET : 클라이언트가 서버에 리소스를 요청할 때 사용 (CRUD에서 Read)
2. POST : 클라이언트가 서버의 리소스를 새로 만들 때 사용 (CRUD 에서 Create)
3. PUT : 클라이언트가 서버의 리소스를 수정 할 때 사용 (CRUD에서 Update: 전체 수정)
4. DELETE: 클라이언트가 서버의 리소스를 삭제 할 때 사용 (CRUD에서 Delete)   

그 외) PATCH, HEAD, OPTIONS, CONNECT, TRACE 등...

## HTTP 상태 코드 정보
- ~~1xx (Infomational): 요청이 수신되어 처리중 (거의 사용 안됨)~~   
- 2xx (Successful): 요청 정상 처리 (200 OK, 201 Created 정도만 사용)   
- 3xx (Redirection): 요청을 완료하여 추가 행동이 필요
- 4xx (Client Error): 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음
- 5xx (Server Error): 서버 오류, 서버가 정상 요청을 처리하지 못함, 서버에 문제가 있기 때문에 재시도하면 성공할 수도 있음 (복구 되었거나 등등)

# HTTPS(Hyper Text Transfer Protocol Secure)
> HTTP에 `SSL` 혹은 `TLS` 프로토콜을 사용한 통신 규약입니다.

## 특징
- :443
- 공개키 암호화 방식

## HTTPS 통신 흐름
1. 클라이언트는 서버에 접속하면, 서버인증서(CA)를 받는다.
2. 서버인증서 신뢰 여부 체크 후, 공개키를 추출한다.
3. 클라이언트는 서버와 통신하는 동안만 사용할 대칭키를 임의로 만들고, 해당 대칭키를 공개키로 암호화 후 전송한다.
4. 서버는 개인키로 클라이언트가 보낸 메시지를 복호화하여 대칭키를 추출하고, 해당 대칭키를 이용하여 클라이언트와 통신한다.
