# Open API(Application Programming Interface) 란?
개방형 API로써 프로그래밍에서 사용할 수 있는 개방되어 있는 상태의 인터페이스를 말한다.

* Daum, Naver 등의 포털 사이트나 통계청, 기상청 등과 같은 관공서에서도 가지고 있는 데이터를 외부 응용 프로그램에서 사용할 수 있도록 Open API를 제공하고 있다
* Open API와 함께 자주 거론되는 기술이 REST이며, 대부분 Open API는 REST 방식으로 지원되고 있다

# REST(REpresentational Safe Transfer)
**HTTP URI + HTTP Method**  
HTTP URI를 통해 제어할 자원(Resource)을 명시하고, HTTP Method(get, post, put delete)를 통해 해당 자원을 제어하는 명령을 내리는 방식의 아키텍쳐

* HTTP 프로토콜에 정의된 4개의 메서드들이 자원에 대한 CRUD Operation을 정의

# Restful API
HTTP와 URI 기반으로 자원에 접근할 수 있도록 제공하는 애플리케이션 개발 인터페이스 (REST의 원리를 따르는 시스템은 RESTful이란 용어로 지칭된다)

# 기존의 웹 접근 방식과 RESTful API 방식과의 차이점
* 기존의 게시판은 GET과 POST만으로 자원에 대한 CRUD를 처리하며, URI는 액션을 나타낸다
* RESTful 게시판은 4가지 메서드를 모두 사용하여 CRUD를 처리하며, URI는 제어하려는 자원을 나타낸다

# 핵심 어노테이션
* Spring MVC에서는 클라이언트에서 전송한 XML이나 JSON 데이터를 Controller에서 Java 객체로 변환해서 받을 수 있는 기능을 제공하고 있다 
* Java 객체를 XML이나 JSON으로 변환해서 전송할 수 있는 기능을 제공하고 있다

## @RequestBody 
클라이언트로부터의 XML이나 JSON 포맷의 데이터를 Java로 변환해준다.   즉, HTTP Request Body를 Java객체로 전달받을 수 있다.


## @ResponseBody
Java 객체를 HTTP Response Body로 전송할 수 있다.  
Java를 클라이언트에게 XML이나 JSON으로 전달한다.