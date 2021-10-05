RequestContextHolder 는 Spring에서 전역으로 Request에 대한 정보를 가져오고자 할 때 사용하는 유틸성 클래스이다.

 

주로, Controller가 아닌 Business Layer 등에서 Request 객체를 참고하려 할 때 사용한다.

Request Param이라던지.. UserAgent 라던지.. 매번 method의 call param으로 넘기기가 애매할 때 주로 쓰인다.

아래처럼 호출하면 같은 Request Thread 범위에 있는 경우 요청정보를 얻어낼 수 있다.