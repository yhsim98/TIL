# SMTP(Simple mail transfer protocol)
시스템 구성 요소
* User Agent : 사용자 장치에서 메일 작성, 읽기, 관리 기능 수행
* Mail Server : 다수 사용자들의 메일박스 관리, 메일 송.수신 제어
* SMTP : 메일 전송 프로토콜
    * TCP를 사용한다

이메일 전송 과정
1. 송신자 UA에서 메일 작성 후 메일 서버로 전달
2. 송신자 메일 서버의 출력 메시지 큐(outgoing message queue)에 저장
3. 수신자 메일 서버로 전송
4. 전송 불가시 30분 단위로 재전송 시도, 정해진 기간 동안 전송 불가시 중단 및 송신자에게 통보
5. 수신자 메일 서버의 수신자 메일박스(mailbox)에 저장
6. 수신자 UA에서 메일 서버의 메일박스의 메일 읽기 및 관리

SMTP  
* 클라이언트-서버 프로토콜
    * 클라이언트 : 송신 메일 서버/UA
    * 서버 : 수신 서버 메일/송신 메일 서버
* TCP 사용
    * 신뢰 전송
    * 서버 포트번호 : 25
* ASCII 텍스트 프로토콜
    * 명령어 : ASCII 그래픽 문자 + 제어문자
        * 제어 정보
    * 메시지 : ASCII 그래픽 문자 + 제어문자

MIME(Multipurpose Internet Mail Extension) : base-64
* 현재는 아스키 코드만이 아닌 한글, 영상 등 여러 포맷으로 보낸다
* 그래서 MIME을 사용한다
* MIME은 송수신 하고자 하는 정보를 아스키 코드로 변환 해준다

## HTTP vs SMTP
HTTP
* Pull Protocol
* Multimedia 객체 전송
* 1 응답 메시지 : 1 웹 객체

SMTP
* Push Protocol
* Text-only 메시지 전송
    * text라는 것은 아스키 문자를 뜻한다
* 1 전송 메시지 : 멀티-파트 메시지




