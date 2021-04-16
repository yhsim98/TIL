# Rest API
Rest 아키텍쳐 스타일을 따르는 API

# REST
분산 하이퍼미디어 시스템을 위한 아키텍쳐 스타일

# 아키텍쳐 스타일 
제약조건의 집합, 제약조건을 모두 지켜야 REST를 따른다고 할 수 있다.

# REST 제약조건
* client-server
* stateless
* cache
* uniform interface
* layered system
* code-on-demand(optional)

uniform interface를 잘 지키지 않는다

# Uniform Interface 제약조건 
* identification of resources
* manipulation of resoure through representations
    * 리소스를 조작할 때(CRUD) http 메소드에 그것을 담아서 보내야 한다
* self-descriptive mesaage
    * 잘 안지킨다
    * 확장 가능한 커뮤니케이션이 가능해진다
        * 서버나 클라이언트가 변경되어도 항상 해석 가능하다
* hypermedia as the engine of application state(HATEOAS)
    * 잘 안지킨다
    * 애플리케이션 상태 전이의 late binding
        * 링크가 동적으로 변경될 수 있다
            * 어떤 상태로 전이가 완료되고 나서야 그 다음 전이될 수 있는 상태가 결정된다

# self-descriptive mesaage
메시지는 스스로를 설명해야 한다는 것

    GET/HTTP/1.1

    ->

    GET/HTTP/1.1
    Host: www.example.org    //목적지


    HTTP/1.1 200 OK
    [ { "op" : "remove" , "path" : "/a/b/c" }]

    ->

    HTTP/1.1 200 OK
    Content-Type: application/json-patch+json    //타입과 명세
    [ { "op" : "remove" , "path" : "/a/b/c" }]

# HATEOS
애플리케이션의 상태는 Hyperlink를 이용해 전이되어야 한다.


# Uniform Interface의 필요성
독집적 진화를 위해서 이다

* 서버와 클라이언트가 각각 독립적으로 진화한다
* 서버의 기능이 변경되어도 클라이언트를 업데이트할 필요가 없다
* REST를 만들게 된 계기 : "How do I improve HTTP without breaking the Web"

# 웹
* 웹 페이지를 변경했다고 웹 브라우저를 업데이트할 필요는 없다
* 웹 브라우저를 업데이트했다고 웹 페이지를 변경할 필요도 없다
* HTTP 명세가 변경되어도 웹은 잘 동작한다
* HTML 명세가 변경되어도 웹은 잘 동작한다

# 상호운영성(interoperability)에 대한 집착
* Referer 라는 오타가 있는데 안고침
* charset 잘못 지은 이름이지만 안 고침
* HTTP 상태 코드 416 포기함 
* HTTP/0.9 아직도 지원함

# REST가 웹의 독집적 진화에 역할
* HTTP에 지속적으로 영향을 줌
* HOST 헤더 추가
* 길이 제한을 다루는 방법이 명시
* URI에서 리소스의 정의가 추상적으로 변경됨 : "식별하고자 하는 무언가"
* 기타 HTTP와 URI에 많은 영향을 줌
* HTTP/1.1 명세 최신판에서 REST에 대한 언금이 들어감

# REST가 성공했는가?
* REST는 웹의 독립적 진화를 위해 만들어졌다
* 웹은 독립적으로 진화하고 있다 -> 성공적

# REST API는?
* REST API는 REST아키텍쳐 스타일을 따라야한다
* 오늘날 스스로 REST API라고 하는 API들의 대부분이 REST아키텍쳐 스타일을 따르지 않는다

# REST API가 모든 제약조건을 지켜야 하는가?
REST API는 하이퍼텍스트를 포함한 self-descriptive한 메시지의 uniform interface를 통해 리소스에 접근하는 API 로써 모든 제약조건을 지켜야 한다.

# 원격 API가 꼭 REST API여야 하는가?
꼭 그럴필요는 없다. 시스템 전체를 통제할 수 있다고 생각하거나, 진화에 관심이 없다면, REST에 대해 따지느라 시간을 낭비하지 마라

서버와 클라이언트를 같이 개발하거나, 업데이트를 하지 않는다거나 등등

현재 Rest API는 엄연히 REST API라 할 수 없다. 

# 웹과 HTTP API의 비교
웹페이지 : Protocol : HTTP,  의사소통 : 사람-기계,  Media Type : HTML  
HTTP API : HTTP 기계-기계 JSON

Media Type이 문제다!!

# HTML과 JSON비교
* HTML
    * 하이퍼링크가 가능하다
    * self-descriptive가 된다
* JOSN
    * 하이퍼링크가 정의되어있지 않다
    * self-descriptive가 불완전하다
        * 문법은 가능하지만 의미를 해석하려면 별도 api문서가 필요하다

# JOSN을 REST하게 바꿔보자
## self-descriptive

1. custiom media type
직접 정의 귀찮다

2. Profile를 정의해서 링크를 건다

* 단점
    * 클라이언트가 Link 헤더와 profile을 이해해햐 한다
    * Content negotiation을 할 수 없다

## HATEOS
본문에 직접 링크를 넣던가, HTTP 헤더를 사용하던가


# 결론
발전해가는 프로그램을 만들고 싶다면 REST하게 해봐라!!