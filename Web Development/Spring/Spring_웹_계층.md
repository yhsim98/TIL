# Spring 웹 계층

![이미지](https://kyu9341.github.io/img/springLayer.png)

* Web Layer
    * 흔히 사용하는 Controller와 JSP / Freemarker 등의 뷰 템플릿 영역
    * 이외에도 필터(@Filter), 인터셉터, 컨트롤러 어드바이스(@ControllerAdvice) 등 *외부 요청과 응답*에 대한 전반적인 영역을 이야기한다
* Service Layer
    * @Service에 사용되는 서비스 영역
    * 일반적으로 Controller와 Dao의 중간 영역에서 사용
    * @Transactional이 사용되어야 하는 영역이기도 하다
* Repository Layer
    * Database와 같이 데이터 저장소에 접근하는 영역
    * Dao(Data Access Object) 영역
* Dtos 
    * Dto(Data Transfer Object)는 *계층 간에 데이터 교환을 위한 객체*를 이야기하며 Dtos는 이들의 영역
    * 예를 들어 뷰 템플릿 엔진에서 사용될 객체나 Repository Layer에서 결과로 넘겨준 객체 등이 이들을 이야기한다
* Domain Model 
    * 도메인이라 불리는 개발 대상을 모든 사람이 동일한 관점에서 이해할 수 있고 공유할 수 있도록 단순화시킨 것을 도메인 모델이라고 한다
    * 택시 앱이라고 하면 배차, 탑승, 요금 등이 모두 도메인이 될 수 있다
    * @Entity가 사용된 영역 역시 도메인 모델이다
    * 무조건 데이터베이스의 테이블과 관계가 있어야 하는 것은 아니다, ,VO처럼 값 객체들도 이 영역에 해당된다



![전체 구조](https://gmlwjd9405.github.io/images/spring-framework/spring-package-flow.png)


# Domain Model
Domain에서 비즈니스 처리를 담당하게 하는 것

Service Layer에서 비즈니스 로직을 처리하게 되면 모든 로직이 서비스 클래스 내부에서 처리되어 서비스 계층이 무의미하며, 객체란 단순히 데이터 덩어리의 역할만 하게 된다
도메인에서 *각자의 처리*를 하고 서비스에서는 *트랜젝션과 도메인 간의 순서*만 보장해 주면 된다

### References
[스프링 부트와 AWS로 혼자 구현하는 웹서비스](http://www.yes24.com/Product/Goods/83849117)