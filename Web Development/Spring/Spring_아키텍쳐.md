# Spring 웹 계층
대부분의 중, 대규모 웹 애플리케이션은 효율적인 개발 및 유지보수를 위해 계층화하여 개발하는 것이 일반적이다.  

사용자관리 프로젝트 아키텍쳐에서 기본적으로 가지는 계층은 Web(Presentation) Layer, Service Layer, Data Access(Repository) Layer 3계층과 모든 계층에서 사용되는 도메인 모델 클래스로 구성되어 있다.

각각의 계층은 계층마다 독립적으로 분리하여 구현하는 것이 가능해야 하며, 각 계층에서 담당해야 할 기능들이 있다. 

각 계층 사이에서는 인터페이스를 이용하여 통신하는 것이 일반적이다.

![이미지](https://kyu9341.github.io/img/springLayer.png)

* Web Layer(presentation Layer)
    * 흔히 사용하는 Controller와 JSP / Freemarker 등의 뷰 템플릿 영역
    * 이외에도 필터(@Filter), 인터셉터, 컨트롤러 어드바이스(@ControllerAdvice) 등 **브라우저상의 웹 클라이언트 즉 외부의 요청과 응답**에 대한 전반적인 영역을 이야기한다
    * 상위계층(서비스, 데이터)에서 발생하는 예외에 대한 처리를 해준다
    * 최종 UI에서 표현해야 할 도메인 모델을 사용한다
    * 최종 UI에서 입력한 데이터에 대한 유효성 검증(Validation) 기능을 제공한다
    * 비즈니스 로직과 최종 UI를 분리하기 위한 컨트롤러 기능을 제공한다
    * @Controller 어노테이션을 사용하여 작성된 Controller 클래스가 이 계층에 속한다
* Service Layer
    * @Service에 사용되는 서비스 영역
    * 애플리케이션의 **비즈니스 로직 처리**와 비즈니스와 관련된 도메인 모델의 적합성 검증 
    * 일반적으로 Controller와 Dao의 중간 영역에서 사용
    * 프리젠테이션 계층과 데이터 엑세스 계층 사이를 연결하는 역할로서 두 계층이 직접적으로 통신하지 않게 하여 애플리케이션의 유연성을 증가시킨다
    * 다른 계층들과 통신하기 위한 인터페이스를 제공한다
    * @Transactional이 사용되어야 하는 영역이기도 하다
* Repository(data access) Layer
    * Database와 같이 데이터 저장소에 접근하는 영역
    * 관계형 데이터베이스의 데이터를 조작하는 데이터 엑세스 로직을 객체화
    * 데이터를 조회, 등록, 수정, 삭제함
    * ORM 프레임워크를 주로 사용하는 계층
    * DAO(Data Access Object) 영역
    * DAO 인터페이스와 @Repository 어노테이션을 사용하여 작성된 DAO 구현 클래스가 이 계층에 속함
* Dtos 
    * Dto(Data Transfer Object)는 *계층 간에 데이터 교환을 위한 객체*를 이야기하며 Dtos는 이들의 영역
    * 예를 들어 뷰 템플릿 엔진에서 사용될 객체나 Repository Layer에서 결과로 넘겨준 객체 등이 이들을 이야기한다
* Domain Model 
    * 도메인이라 불리는 개발 대상을 모든 사람이 동일한 관점에서 이해할 수 있고 공유할 수 있도록 단순화시킨 것을 도메인 모델이라고 한다
    * 택시 앱이라고 하면 배차, 탑승, 요금 등이 모두 도메인이 될 수 있다
    * @Entity가 사용된 영역 역시 도메인 모델이다
    * 무조건 데이터베이스의 테이블과 관계가 있어야 하는 것은 아니다, DTO, VO처럼 값 객체들도 이 영역에 해당된다



![전체 구조](https://gmlwjd9405.github.io/images/spring-framework/spring-package-flow.png)


# Domain Model
Domain에서 비즈니스 처리를 담당하게 하는 것

Service Layer에서 비즈니스 로직을 처리하게 되면 모든 로직이 서비스 클래스 내부에서 처리되어 서비스 계층이 무의미하며, 객체란 단순히 데이터 덩어리의 역할만 하게 된다
도메인에서 **각자의 처리**를 하고 서비스에서는 **트랜젝션과 도메인 간의 순서**만 보장해 주면 된다


# 클래스 역할

## 프레젠테이션 계층 
### UserController 클래스
* UI 계층과 서비스 계층을 연결하는 역할을 하는 클래스
* JSP에서 UserController를 통해서 서비스 계층의 UserService를 사용하게 된다
* 서비스 계층의 UserService 인터페이스를 구현하나 객체를 IoC컨테이너가 주입해 준다

## 서비스 계층
### UserService 인터페이스
* 서비스 계층에 속한 상위 인터페이스
### UserServicelmpl 클래스
* UserService 인테페이스를 구현한 클래스
* 복잡한 업무 로직이 있을 경우에는 이 클래스에서 업무 로직을 구현하면 된다 
* repository 계층의 UserDao 인터페이스를 구현한 객체를 IoC컨테이너가 주입해 준다

## 데이터 엑세스 계층
### UserDao 인터페이스
* 데이터 엑세스 계층에 속한 상위 인터페이스
### UserDaolmplJDBC 클래스
* UserDao 인터페이스를 구현한 클래스로 이 클래스에서는 데이터 액세스 로직을 구현하면 된다
* SpringJDBC를 사용하는 경우에는 DataSource를 IoC컨테이너가 주입해 준다
* MyBatis를 사용하는 경우에는 SqlSession을 IoC컨테이너가 주입해 준다
### References
[스프링 부트와 AWS로 혼자 구현하는 웹서비스](http://www.yes24.com/Product/Goods/83849117)
