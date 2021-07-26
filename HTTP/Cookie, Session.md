# 들어가기전
HTTP 프로토콜은 Stateless(무상태성), Connectionless(무연결성)이다. 

* Connectionless : Client가 Request를 보내면 Server가 처리해서 Response를 한 뒤 접속을 끊는다
* Stateless : Server가 Client에 관한 정보를 유지하지 않는다

상태를 유지하지 않기 때문에 로그인 등의 기능을 위해서 모든 요청에 사용자 정보를 넘겨야 하는 등 문제가 있었다.

이것을 해결하기 위해 나온것이 Cookie와 Session이다.    
 

# Cookie
* Set-Cookie : 서버에서 클라이언트로 쿠키 전달(응답)
* Cookie : 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

Http 요청시 서버는 Set-Cookie를 헤더에 넣어 응답한다.   브라우저가 그것을 받으면 해당 쿠키를 쿠키 저장소에 저장하게 된다. 참고로 쿠키 하나당 최대 4kb이다.

브라우저가 쿠키를 보낸 서버에 요청을 보내게 되면 쿠키 저장소에 있는 해당 서버에서 보낸 쿠키를 헤더의 Cookie에 넣어 요청을 보낸다.


* 사용처
    * 사용자 로그인 세션 관리
    * 광고 정보 트래킹
        * 하루동안 보지 않음 등등
* 쿠키 정보는 항상 서버에 전송됨
    * 네트워크 트래픽 추가를 유발한다
    * 그래서 최소한의 정보만 사용해야 한다
        * 세션 id, 인증 토큰 등등
    * 서버에 전송하지 않고, 웹 크라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지를 사용한다
* 주의
    * 타인이 들여다보기 쉽기 때문에 보안에 민감한 데이터는 저장하면 안된다
        * 주민번호, 신용카드 번호 등등


# Cookie 구성요소
## Cookie-생명주기
쿠키의 수명을 정할 수 있다.
* expire=date
    * 만료일이 되면 쿠키 삭제한다
* max-age
    * 수명을 직접적으로 정할 수 있다
    * 0이나 음수를 지정하면 쿠키가 삭제된다
* 세션 쿠키 : 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
* 영속 쿠키 : 만료 날짜를 입력하면 해당 날짜까지 유지

## Cookie-도메인
쿠키에 도메인을 명시할 수 있다
* 명시한 문서 기준 도메인 + 서브 도메인 포함된다

만약 생략한다면 현재 문서 기준 도메인에만 적용된다

## Cookie-경로
경로도 지정할 수 있다
* 경로를 지정하면 해당 경로를 포함한 하위 경로 페이지만 쿠키에 접근한다
* 일반적으로 path=/ 루트로 지정한다
* ex)
    * path=/home
    * path=/home/level1 등등

## Cookie-보안
Secure, HttpOnly, SameSite 

* Secure
    * 쿠키는 http, https를 구분하지 않고 전송
    * Secure를 적용하면 https인 경우에만 전송
* HttpOnly
    * XSS 공격 방지
    * 자바스크립트에서 접근 불가
    * HTTP 전송에만 사용
* SameSite
    * XSRF 공격 방지
    * 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송



# Session
cookie는 브라우저의 쿠키 저장소에 저장이 된다면 Session은 서버에 데이터를 저장한다. Session은 Cookie처럼 사용자를 식별하고 정보를 저장하는데 사용되지만, 서버에 저장되므로 Cookie와 달리 정보가 노출되지 않는다.

* 서버에 데이터 저장
* Cookie에 비해 안전하다


Session에는 어떤 사용자의 Session인지 식별하는 고유한 ID를 가지고 있다. 이것을 **Session id**라고 한다.

서버에서 HTTP Request를 받았을 때 Session을 생성할 필요가 있다면 Session과 Session id를 생성하고 setCookie로 Session id를 담아 클라이언트에 보내게 된다. 

서버에서는 Session과 Session id를 데이터베이스, 메모리 등에 저장하고, 클라이언트가 Request를 보낼 때마다 Session ID를 Cookie에 담아 보내면 Session 저장소에서 해당 Session ID에 해당하는 데이터를 찾아 Response에 담아 보낸다.

Session ID를 쿠키로 보내게 되는데 탈취당할 수 도 있다. 따라서 Session ID의 생성, 폐기 시간을 짧게 정하는 등 추가적인 보안 대책이 필요하다.