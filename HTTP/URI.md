# URI(Uniform Resource Identifier)
URI는 로케이터, 이름 또는 둘다 추가로 분류될 수 있다.  
URL(Reosurce Locator)와 URN(Reosurce Name)을 아우르는 큰 개념.

* Uniform : 리소스 식별하는 통일된 방식
* Reosurce : 자원, URI로 식별할 수 있는 모든 것(제한없음)
* Identifier : 다른 항목과 구분하는데 필요한 정보

* URL : Uniform Resource Locator
* URN : Uniform Reource Name

# URL, URN
* URL : 리소스가 있는 위치를 지정
* URN : 리소스에 이름을 부여
* 위치는 변할 수 있지만, 이름은 변하지 않는다
* URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음
* 앞으로는 URI를 URL과 같은 의미로 사용하겠습니다

# URL 분석
    scheme://[userinfo@]host[:port][/path][?query][#fragment]

## scheme
* 주로 프로토콜에 사용
* 프로토콜 : 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙
    * http, https 등
* http는 80포트, https는 443 포트를 주로 사용(디폴트값), 포트는 생략 가능
* https는 http에 보안 추가 (HTTP Secure)

## userinfo
* URL에 사용자 정보를 포함해서 인증
* 거의 사용하지 않음

## port
* 접속 포트
* 일반적으로 생략, 생략시 http는 80, https는 443

## path
* 리소스 경로(path), 계층적 구조
ex)
* /home/file1.jpg
* /members
* /members/100

## query
* key=value 형태
* ?로 시작, &로 추가 가능 ?keyA=valueA&keyB=valueB
* query parametor, query string 등으로 불림, 웹서버에 제공하는 파라미터, 문자 형태

## fragment
* 페이지 내부 특정 위치로 이동하고자 할 때
* html 내부 북마크 등에 사용
* 서버에 전송하는 정보 아님
