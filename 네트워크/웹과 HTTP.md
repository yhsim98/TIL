# HTTP

# 웹 서비스 모델
웹 서버(Web server)
* 웹 페이지들의 저장소와 요청 처리 소프트웨어

웹 페이지
* 기본 객체(base object)와 참조 객체(object)들로 구성
* 기본 객체 : HTML file, 페이지 내의 다른 객체를 URL(하이퍼링크)로 참조
* 참조 객체 : HTML file, JPEG image, Java applet, audio file, video file ......
    * 서로 다른 서버에 존재가능

웹 객체 주소 : URL(Uniform Resource Locatior)
* host name과 path name으로 구성
    * host name은 서버 주소
    * 그 서버의 지정 웹페이지 경로 주소
    * host name : www.koreatech.ac.kr
    * path name : /kor/main.do


웹 브라우저 
* 웹 서비스 사용자 인터페이스
* 웹 페이지 요청 및 응답 페이지 디스플레이


HTTP(Hyper Text Transfer Protocol)
* 웹 브라우저와 웹 서버 간의 요청 정보와 응답 정보 교환 규칙 정의

TCP Connection
* 웹 요청 정보와 응답 정보의 신뢰 전송 통로


# HTTP 원리
HTTP 요청과 응답
* 웹 객체 각각에 대한 요청과 응답 메시지

Http Request
* 웹 사용자의 요청(URL 입력 또는 하이퍼링크 클릭)으로 웹 브라우저에 의해 생성되는 메시지 
* 웹 서버의 웹 객체 URL과 해당 웹 객체 처리 방식 정보 제공
* 하위 계층의 TCP 연결을 통해 웹 서버에게 전송

HTTP response
* 웹 브라우저의 요청으로 웹 서버에 의해 생성되는 메시지
* 수신한 URL에 해당되는 웹 객체와 웹 객체 속성 정보 제공
* 하위 계층의 TCP 연결을 통해 웹 서버에게 전송

비상태형 프로토콜(stateless protocol)
* HTTP request 메시지와 HTTP response 메시지 간의 관계 정보가 웹 서버에 저장되니 않음 
* 서버는 수신되는 HTTP request 메시지 간의 관계 추론 불가
* 웹 브라우저-웹 서버간의 통신 상태 정보를 유지하지 않음(stateless protocol)

# HTTP와 TCP 연결
TCP 연결과 HTTP 요청
* HTTP request 전 핸드쉐이킹 과정이 있다
* 3 way hand shaking
1. 클라이언트의 연결요청 메시지
    * SYN
2. 서버의 응답 메시지
    * SYN, ACK
3. 클라이언트가 받았다고 다시 응답
4. 이후 HTTP Request

# 비지속 연결(non-persistect) HTTP
비지속 연결 HTTP
* 웹 객체를 위한 HTTP request와 HTTP response 메시지 쌍마다 별도의 TCP 연결 설정
    * 이것을 비지속 연결이라 한다
* 10개의 객체로 구성된 웹 페이지 전송을 위해 10개의 TCP 연결 설정
* 다중 연결(multiple connections) 설정으로 병렬 전송 가능
* 서버 자원 관리 차원에서 클라이언트별 병렬 연결 수 제한(5 ~ 10)

비지속 HTTP와 지연시간
* 객체별 지연시간 : 2RTT + 객체파일 전송시간

# 지속 연결 HTTP
지속 연결 HTTP
* 동일 서버의 다수 웹 객체가 하나의 TCP 연결을 통해 클라이언트에게 전송하도록 TCP 연결 유지
* 일정 시간 동안 사용하지 않으면 TCP 연결 해제
* TCP 연결 지연시간 절약, 사용하지 않는 시간 동안 자원(소켓 자료구조) 낭비
* 다수의 객체를 한꺼번에 요청하고 응답하는 파이프라이닝(pipelining) 적용 가능
    * 한번에 여러개 요청하고 받음으로써 지연시간 줄임

# 지속비지속 연결 장단점 비교
지속 연결 HTTP 
* TCP 연결 지연시간 회피
* 파이프라이닝 지원 가능
* 사용하지 않는 시간 동안 TCP 연결 자원 낭비

비지속 연결 HTTP 
* 필요할 때만 TCP 연결 - 자원(소켓 자료구조) 낭비 회피
* 병렬 TCP 연결 지원 가능 - 제한적
* TCP 연결 지연시간 발생

하이브리드 HTTP 가능



# HTTP 메시지
* HTTP Request
* HTTP Response

# HTTP 요청 메시지 포맷
status line, header line, blank line, entity body

Methods
* GET : body 정보 없이 객체 요청(필요시 URL에 포함시켜 입력 정보 전달)
* POST : body 입력 정보와 함께 객체 요청(form field로 입력)
* HEAD : 헤더 정보만 입력
* DELETE : 파일 삭제

# HTTP 응답 메시지 포맷
요청과 거의 비슷하다

# 웹 쿠키(cookie)
쿠키 사용 시나리오
1. 클라이언트에서 요청이 들어오면 서버는 브라우저를 특정할 수 있는 unique한 id를 만듬
2. 브라우저는 해당 id를 보관, 이전에 한 행위를 보관
3. 클라이언트에서 서버로 요청이 들어갈 때 헤더에 쿠키번호를 보냄
4. 서버는 쿠키를 보고 해당 브라우저가 이전에 어떤 행위를 했는 지 알 수 있다

웹 쿠키 필요성 
* 비상태형 HTTP에 상태형 서비스 구현

웹 쿠키 응용
* 쇼핑 카트 서비스
* 상품 추천 서비스
* 인증 및 인가 서비스
* 기타

프라이버시 문제
* 웹 서버에 개인 정보 노출

# 웹 캐시(Web Cache) : Proxy server
웹 캐시
* 원래의 웹 서버들을 대신하여 HTTP 요청 메시지를 처리하는 중간 서버
* 대상 웹 브라우저의 HTTP 요청 메시지를 웹 캐시로 방향 전환(redirect)
* 웹 캐시에 요청된 객체가 존재하면 웹 브라우저에 전송
* 웹 캐시에 요청된 객체가 존재하지 않으면 웹 캐시가 원래의 웹 서버에 요청 메시지를 보내 응답 메시지를 수신
* 웹 캐시가 웹 서버로부터 수신한 객체를 자신의 서버에 저장하고 웹 브라우저로 전송
* 주로 Enterprise ISP, Residential ISP에 사용

웹 캐시 사용의 장점
* 응답 지연시간 단축
* 네트워크 트래픽 감축
* 보안목적
    * 프록시를 경유하기 때문에 모니터링 하기 편하다

조건부 GET(Conditional GET)
* 요청 메시지 헤더에 지정된 시간 이후에 수정된 객체만 다운로드
* 객체가 웹 캐시에 저장된 이후에 원래의 웹 서버에서 갱신되었는지 확인 가능
* 웹 캐시에 저장된 객체의 최신화에 유용





















