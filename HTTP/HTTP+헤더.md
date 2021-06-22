# HTTP 헤더
요청이나 응답에서 전달할 실제 데이터를 표현(representation)이라고 한다. 

Representation은 representation Metadata + Representation Data이다.

HTTP헤더는 representation metadata을 포함하여 representation data를 해석할 수 있는 정보를 제공한다
* 데이터 유형(html, json), 데이터 길이, 압축 정보 등등


# 표현(Representation)
표현에 대한 여러가지 정보가 있다. 이런 정보를 표현 헤더에 저장한다.

* Content-Type : 표현 데이터 형식
    * text/html; charset=utf-8
    * application/json
    * image/png
* Content-Encoding : 표현 데이터 압축 방식
    * 표현 데이터를 압축하기 위해 사용
    * 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
    * 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
    * gzip, deflate, identity 등등
* Content-Language : 표현 데이터의 자연 언어
* Content-Length : 표현 데이터의 길이
    * 바이트 단위





# 인증
* Authorization : 클라이언트 인증 정보를 서버에 전달

## WWW-Authenticate
* 리소스 접근시 필요한 인증 방법 정의
* 401 Unauthorized 응답과 함께 사용


# 쿠키
* Set-Cookie, Cookie 이렇게 2개의 헤더를 사용한다
* Set-Cookie : 서버에서 클라이언트로 쿠키 전달(응답)
* Cookie : 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

* 서버가 set-Cookie로 헤더에 key-value쌍의 값을 보내면 웹브라우저가 받아 쿠키 저장소에 cookie로 저장하게 된다.
* 웹 브라우저가 서버에 요청을 보낼 때 자동으로 쿠키 저장소의 cookie를 헤더에 담아 서버에 같이 보내게 된다


* 서버가 set-Cookie를 할 때 권안, 생명주기 등등을 cookie로 저장한다
* 사용처
    * 사용자 로그인 세션 관리
    * 광고 정보 트래킹
* 쿠키 정보는 항상 서버에 전송됨
    * 네트워크 트래픽 추가 유발
    * 최소한의 정보만 사용해야 한다(세션id, 인증 토큰 등)
    * 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지를 사용하면 된다
* 주의!
    * 보안에 민감한 데이터는 저장하면 안된다


## 쿠키 - 생명주기
* Expires, max-age
    * 예)expires=Sat, 25-Dec-2020 04:39:21 GMT
        * 만료일이 되면 쿠키가 삭제된다
    * 예)Set-Cookie: max-age=3600
        * 0이나 음수를 지정하면 쿠키 삭제
* 세션 쿠키 : 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
* 영속 쿠키 : 만료 날짜를 입력하면 해당 날짜까지 유지


## 쿠키 - 도메인
* 예) domain=example.org
* 명시 : 명시한 문서 기준 도메인 + 서브 도메인 포함
    * domain=example.org를 지정해서 쿠키 생성했을 경우
    * example.org는 물론이고
    * dev.example.org도 쿠키 접근
* 생략 : 현재 문서 기준 도메인만 적용
    * example.org에서 쿠키를 생성하고 domain 지정을 생략했을 경우
    * example.org에서만 쿠키 접근이 가능하고
    * dev.example.org는 쿠키 미접근


## 쿠키 - 경로
* 예) path=/home
* 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
* 일반적으로 path=/루트로 지정


## 쿠키 - 보안
* Secure
    * 쿠키는 http, https를 구분하지 않고 전송
    * Secure를 적용하면 https인 경우에만 전송
* HttpOnly
    * XSS 공격 방지
    * 자바스크립트에서 접근 불가(document.cookie)
    * HTTP 전송에만 사용
* SameSite
    * XSRF 공격 방지
    * 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송