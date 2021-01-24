# IoC(Inversion of Control)
IoC(제어권의 역전)이란, 객체의 생성, 생명주기의 관리까지 모든 객체에 대한 제어권이 바꿔었다는 것을 의미한다.

컴포넌트간의 의존관계(componenet dependency resolution) 결정, 설정(configuration) 및 생명주기(lifecycle)를 해결하기 위한 디자인 패턴

Ioc가 아닌 경우 개발자가 직접 new 등을 사용해서 객체를 생성해야 하지만 Ioc인 경우 F/W을 통해 container가 생성해서 개발자의 코드에 주입해준다.

# IoC 컨테이너
스프링 프레임워크도 **객체에 대한 생성 및 생명주기를 관리**할 수 있는 기능 즉 IoC 컨테이너 기능을 제공하고 있다.

* IoC 컨테이너는 객체의 생성을 책임지고, 의존성을 관리한다
* POJO의 생성, 초기화, 서비스, 소멸에 대한 권한을 가진다
* 개발자들이 직접 POJO를 생성할 수 있지만 컨테이너에게 맡긴다

Spring 컨테이너가 관리하는 객체를 bean 객체라 한다
# IoC의 분류
IoC는 DL(Dependency Lookup)과 DI(Dependency Injection)로 분류된다  
DL은 특정 컨테이너에 종속되는 api를 쓰기 때문에 주로 DI를 쓴다
