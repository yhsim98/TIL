# Spring Framework
Java 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크

# Spring Framework 전략
엔터프라이즈 개발의 복잡함을 상대하는 Spring의 4가지 전략 
* Portable Service Abstraction, DI, AoP, POJP

# 스프링 삼각형 
엔터프라이즈 개발의 복잡함을 상대하는 Spring의 전략
* Portable Service Abstraction, DI, AOP, POJO 

## Portable Service Abstraction(서비스의 추상화)
트랜잭션 추상화, OXM(object, XML, mapping) 추상화, 데이터 액세스의 Exception 변환기능 등 기술적인 복잡함은 추상화를 통해 Low Level의 기술 구현 부분과 기술을 사용하는 인터페이스로 분리한다.

* 비즈니스 로직의 구현과 그 사용을 분리한다

## DI(Dependency Injection) 
Spring은 객체지향에 충실한 설계가 가능하도록 단순한 객체형태로 개발할 수 있고, DI는 유연하게 확장 가능한 객체를 만들어 두고 그 관계는 외부에서 동적으로 설정해 준다

* 객체와 객체와의 관계를 컨테이너가 동적으로 설정해 준다

## AOP(관점지향프로그래밍)
AOP는 애플리케이션 로직을 담당하는 코드에 남아 있는 기술 관련 코드를 분리해서 별도의 모듈로 관리하게 해주는 강력한 기술

## POJO
POJO는 객체지향 원리에 충실하며, 특정 환경이나 규약에 종속되지 않고 필요에 따라 재활용될 수 있는 방식으로 설계된 객체이다.


# Spring Framework의 특징
* 컨테이너 역할
    * Spring 컨테이너는 Java 객체의 LifeCycle을 관리하며, 사용자는 Spring 컨테이너로 부터 필요한 객체를 가져와 사용만 하면 된다
* DI 지원
    * Spring은 설정 파일이나 어노테이션을 통해서 객체 간의 의존관계를 설정할 수 있도록 하고 있다
* AOP 지원
    * Spring은 트랜잭션이나 로깅, 보안과 같이 공통적으로 필요로 하는 모듈들을 실제 비즈니스 로직에서 분리해서 적용할 수 있다
* POJO 지원
    * Spring 컨테이너에 저장되는 Java객체는 특정한 인터페이스를 구현하거나, 특정 클래스를 상속받지 않아도 된다
* 트랜잭션 처리를 위한 일관된 방법을 지원
    * JDBC, JTA 등 어떤 트랜잭션을 사용하던 설정을 통해 정보를 관리하므로 트랜잭션 구현에 상관없이 동일한 코드 사용 가능 
* 영속성과 관련된 다양한 API 지원
    * Spring은 MyBatis, Hibernate 등 데이터베이스 처리를 위한 ORM 프레임워크들과의 연동 지원

# Spring Framework를 구성하는 기능 요소
![기능 요소](data:image/png;base64,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)

## Spring Core 컨테이너
* Spring 프레임워크의 기본기능을 제공한다
    * 주로 스프링 컨테이너 기능을 하는 클래스들로 구성되어 있다
* 이 모듈에 있는 BeanFactory는 Spring의 기본 컨테이너이면서 스프링 DI의 기반이다

## Spring AOP
* AOP 모듈을 통해 관점 지향 프로그래밍을 지원
* AOP 모듈은 스프링 어플리케이션에서 Aspect를 개발할 수 있는 기반을 지원한다

## Spring ORM
* ORM 프레임워크와의 연동을 제공
    * ORM 제품들을 Spring의 기능과 조합해서 사용할 수 있도록 해준다

## Spring DAO
* JDBC에 대한 추상화 계층으로 JDBC 코딩이나 예외처리 하는 부분을 간편화 시켰으며, AOP 모듈을 이용해 트랜잭션 관리 서비스도 제공한다

## Spring Web
* 일반적인 웹 어플리케이션 개발에 필요한 기본기능을 제공한다

## Spring Context
* Spring Core기능을 확장한 것으로 국제화 기능(언어), 어플리케이션 life cycle 이벤트, 유효성 검증 등을 지원한다

## Spring Web MVC(Model, View, Controller)
* 사용자 인터페이스가 어플리케이션 로직과 분리되는 웹 어플리케이션을 만드는 경우에 일반적으로 사용되는 패러다임