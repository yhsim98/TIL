# JMeter
성능 측정 및 부하 테스트 기능을 제공하는 오픈 소스 자바 에플리케이션

다양한 형태의 애플리케이션 테스트 지원
* 웹 - HTTP, HTTPS
* SOAP / REST 웹 서비스
* FTP
* 데이터베이스(JDBC) 사용
* Mail
* .....

CLI 지원
* CI 또는 CD 툴과 연동할 때 편리함
* UI 사용하는 것보다 메모리 등 시스템 리소스를 적게 사용

주요 개념
* Thread Group : 한 쓰레드 당 유저 한명
* Sampler : 어떤 유저가 해야 하는 액션
* Listner : 응답을 받았을 할 일
* ....

## 사용법
* Number of Threads
    * 가상의 생성자를 몇 명으로 설정할 것인지
* Ramp-up period
    * 한 번의 실행을 몇 초동안 완료 시킬 것인지
* Loop Count
    * 반복하고자 하는 횟수

결과에 대한 여러 통계 같은 것도 제공한다

이를 통해 서버 성능을 가지석으로 볼 수 있다