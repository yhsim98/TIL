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


















