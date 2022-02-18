# 스프링 시큐리티
막강한 인증과 인가 기능을 가진 프레임워크이다.

spring security는 인증과 인가를 Filter 기반으로 동작하기 때문에 Spring MVC와 분리 및 동작한다.  

Spring Security는 보안과 관련해서 체계적으로 많은 옵션을 제공하기 때문에 개발자들이 일일이 보안 관련 로직을 작성하지 않아도 된다.

# 보안관련 용어
* 접근 주체(Principal) : 보호된 대상에 접근하는 유저
* 인증(Authenticate) : 해당 유저가 누구인지 확인(로그인 등)
    * 애플리케이션의 작업을 수행할 수 있는 주체임을 증명
* 인가(Authorize) : 현재 유저가 어떤 서비스, 페이지에 접근할 수 있는 권한이 있는지 검사
* 권한 : 인증된 주체가 애플리케이션의 동작을 수행할 수 있도록 허락되었는지를 결정
    * 권한 승인이 필요한 부분으로 접근하려면 인증 과정을 통해 주체가 증명되어야 한다
    * 권한 부여에도 두가지 영역이 존재하는데 웹 요청 권한, 메소드 호출 및 도메인 인스턴스에 대한 권한 부여

# spring security의 구조
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile23.uf.tistory.com%2Fimage%2F99A7223C5B6B29F003F5F0)

spring security는 세션-쿠키방식으로 인증한다.

JWT 등 토큰인증방식은 spring-security-oauth2를 사용하면 된다.

인증과정을 살펴보면
1. 유저가 로그인 시도(http request)
2. AuthenticationFilter 에서부터 위와 같이 user DB까지 들어감
3. DB에 있는 유저라면 UserDetais로 꺼내서 유저의 session 생성
4. spring security의 인메모리 세션저장소인 SecurityContextHolder에 저장
5. 유저에게 session ID와 함께 응답을 내려줌
6. 이후 요청에서는 요청 쿠키에서 JSESSIONID를 확인해 검증 후 유효하면 Authentication을 부여한다

# 