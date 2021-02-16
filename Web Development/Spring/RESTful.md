# Open API(Application Programming Interface) 란?
개방형 API로써 프로그래밍에서 사용할 수 있는 개방되어 있는 상태의 인터페이스를 말한다.

* Daum, Naver 등의 포털 사이트나 통계청, 기상청 등과 같은 관공서에서도 가지고 있는 데이터를 외부 응용 프로그램에서 사용할 수 있도록 Open API를 제공하고 있다
* Open API와 함께 자주 거론되는 기술이 REST이며, 대부분 Open API는 REST 방식으로 지원되고 있다

# REST(REpresentational Safe Transfer)
**HTTP URI + HTTP Method**  
HTTP URI를 통해 제어할 자원(Resource)을 명시하고, HTTP Method(get, post, put delete)를 통해 해당 자원을 제어하는 명령을 내리는 방식의 아키텍쳐

* HTTP 프로토콜에 정의된 4개의 메서드들이 자원에 대한 CRUD Operation을 정의