# 핵심기능과 부과기능
* 업무 로직을 포함하는 기능을 핵심 기능(Core Concerns)라 한다
* 핵심 기능을 도와주는 부가적인 기능을 부가기능 이라고 부른다
* 객체지향의 기본 원칙을 적용하여도 핵심기능에서 부가기능을 분리해서 모듈화하는 것은 매우 어렵다

# AOP(Aspect Oriented Programming)
AOP는 애플리케이션에서의 **관심사의 분리(기능의 분리)** 즉, 핵심적인 기능에서 부가적인 기능을 분리한다. 분리한 부가기능을 Aspect라는 독특한 모듈형태로 만들어서 설계하고 개발하는 방법

* OOP를 적용하여도 핵심기능에서 부가기능을 **쉽게 분리된 모듈로 작성하기 어려운 문제점을** AOP가 해결해 준다고 볼 수 있다
* AOP는 부가기능을 Aspect로 정의하여, **핵심기능에서 부가기능을 분리함으로써** 핵심기능을 설계하고 구현할 때 **객체지향적인 가치**를 지킬 수 있도록 도와주는 개념이다

# Aspect
부가기능을 정의한 코드인 어드바이스(advice)와 어드바이스를 어디에 적용하는지를 결정하는 포인트컷(pointCut)을 합친 개념이다.

    Advice + PointCut = Aspect

* AOP 개념을 적용하면 핵심기능 코드 사이에 침투된 부가기능을 독립적인 Aspect로 구분해 낼 수 있다
* 구분된 부가기능 Aspect를 런타임 시에 필요한 위치에 동적으로 참여하게 할 수 있다

# AOP 용어

## Target
핵심기능을 담고 있는 모듈로, 타겟은 부가기능을 부여할 대상이 된다.

## Advice
타겟에 제공할 부가기능을 담고 있는 모듈이다.

## Join Point
어드바이스가 적용될 수 있는 위치를 말한다. 즉, 타겟 객체가 구현한 인터페이스의 모든 메서드는 조인 포인트가 된다.

## PointCut
어드바이스를 적용할 타겟의 메서드를 선별하는 정규표현식이다.  
포인트컷 표현식은 execution으로 시작하고, 메서드의 Signature를 비교하는 방법을 주로 이용한다.

## Aspect
AOP의 기본 모듈이다. Aspect는 싱글톤 형태의 객체로 존재하게 된다.

## Advisor
Advice와 PointCut을 합친 것이다. Spring AOP에서만 사용되는 특별한 용어이다.

## Weaving
위빙은 포인트컷에 의해서 결정된 타겟의 조인 포인트에 부가기능을 삽입하는 과정을 뜻한다.  

위빙은 AOP가 핵심기능의 코드에 영향을 주지 않으면서 필요한 부가기능을 추가할 수 있도록 해주는 핵심적인 처리과정이다.

# Spring AOP의 특징
* 스프링은 프록시 기반 AOP를 지원한다
    * Spring은 타겟 객체에 대한 프록시를 만들어 제공한다
    * 타겟을 감싸는 프록시는 실행기간에 생성된다
    * 프록시는 어드바이스를 타겟 객체에 적용하면서 생성되는 객체이다
* 프록시가 호출을 가로챈다
    * 프록시는 타겟 객체에 대한 호출을 가로챈 다음 어드바이스의 부가기능 로직을 수행하고 난 후에 타겟의 핵심기능 로직을 호출한다.(전처리 어드바이스)
    * 또는 타겟의 핵심기능 로직 메서드를 호출한 후에 부가기능을 수행하는 경우도 있다.(후처리 어드바이스)
* Spring AOP는 메서드 조인 포인트만 지원한다
    * Spring은 동적 프록시를 기반으로 AOP를 구현함으로 메서드 조인 포인트만 지원한다. 즉, 핵심기능(타겟)의 메서드가 호출되는 런타임 시점에만 부가기능(어드바이스)을 적용할 수 있다
    * 반면에 AspectJ 같은 고급 AOP 프레임워크를 사용하면 객체의 생성, 필드값의 조회와 조작, static 메서드 호출 및 초기화 등의 다양한 작업에 부가기능을 적용할 수 있다

# Spring AOP 구현 방식
* XML 기반의 POJO 클래스를 이용한 AOP 구현
    * 부가기능을 제공하는 Advice 클래스를 작성한다
    * XML 설정 파일에 aop:config 태그를 이용해서 aspect를 설정한다
* @Aspect 어노테이션을 이용한 AOP 구현
    * @Aspect 어노테이션을 이용해서 부가기능을 제공하는 Aspect 클래스를 작성한다. 이때 Aspect 클래스는 어드바이스를 구현하는 메서드와 포인트컷을 포함한다
    * **XML 설정 파일에 aop:aspectj-autoproxy/** 를 설정한다


# Advice 클래스 종류

* Around 어드바이스
    * 타겟의 메서드가 호출되기 이전 시점과 이후 시점에 모두 처리해야 할 필요가 있는 부가기능을 정의한다
* Before 어드바이스
    * 타겟의 메서드가 실행되기 이전 시점에 처리해야 할 필요가 있는 부가기능을 정의한다
* After Returning 어드바이스
    * 타겟의 메서드가 정상적으로 실행된 이후 시점에 처리해야 할 필요가 있는 부가기능을 정의한다
* After Throwing 어드바이스
    * 타겟의 메서드가 예외를 발생된 이후 시점에 처리해야 할 필요가 있는 부가기능을 정의한다
    * 예외가 던져질 때 실행되는 Advice

# Advice를 정의하는 태그
* aop:before
    * 메서드 실행 전에 적용되느 어드바이스를 정의
* aop:after-returning 
    * 메서드가 정상적으로 실행된 후에 적용되는 어드바이스를 정의
* aop:after-throwing 
    * 메서드가 예외를 발생시킬 때 적용되는 어드바이스를 정의한다
    * try-catch 블록에서 catch와 비슷하다
* aop:after 
    * 메서드가 정상적으로 실행되는지 또는 예외를 발생시키는지 여부에 상관없이 어드바이스를 정의한다
    * try-catch-finally에서 finally 블록과 비슷하다
* aop:around
    * 메서드 호출 이전, 이후, 예외발생 등 모든 시점에 적용 가능한 어드바이스를 정의한다


# JoinPoint 인터페이스
JoinPoint는 Spring AOP 혹은 AspectJ에서 AOP가 적용되는 지점을 뜻한다.

해당 지점을 AspectJ에서 JoinPoint라는 인터페이스로 나타낸다.

JoinPoint 인터페이스는 여러 메서드를 제공한다.
* getArgs()
    * 메서드 아규먼트 반환
* getThis() 
    * 프록시 객체를 반환
* getTarget()
    * 대상 객체를 반환
* getSignature()
    * 어드바이즈 되는 메서드의 설명을 반환
* toString()
    * 어드바이즈 되는 메서드의 설명을 출력

모든 어드바이스는 org.aspectj.lang.JoinPoint 타입의 피라미터를 어드바이스 메서드에 첫 번째 매개변수로 선언할 수 있다.

Around 어드바이스는 JoinPoint의 하위 클래스인 ProceedingJoinPoint 타입의 파라미터를 필수적으로 선언해야 한다.

# PointCut 표현식 문법
* AspectJ 포인트컷 표현식은 포인트컷 지시자를 이용하여 작성한다
* 포인트컷 지시자 중에서 가장 대표적으로 사용되는 것은 execution()이다
* execution() 지시자를 사용한 포인트컷 표현식의 문법구조는 다음과 같다

![포인트컷표현식문법](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb3xxfA%2FbtqCMn5Y608%2F3K273easbaK1lkK4D4oRSk%2Fimg.png)

* 표현식 예시

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbcOoV1%2FbtqCNaSGVgF%2F9MvjvCMGPYbeplEQ1aihIk%2Fimg.png)

ex) execution(* hello(..))
hello라는 이름을 가진 메소드를 선정(..은 파라미터는 모든 종류를 허용)

ex) execution(* hello())
hello 메서드 중 파라미터가 없는 것만 허용

ex) execution( * myspring.user.service.UserServiceImpl.*(..))
myspring.user.service.UserServiceImpl 클래스를 직접 지정하여 해당 클래스의 모든 메서드 지정

ex) execution( * myspring.user.service.*.*(..))
myspring.user.service 패키지의 모든 클래스에 적용(서브패키지의 클래스는 미포함)

ex) execution(* myspring.user.service..*.*(..))
myspring.user.service 패키지와 서브패키지의 모든 클래스를 포함

ex) execution(* *..Target.*(..))
Target이라는 이름의 모든 클래스에 적용(다른 패키지에 같은 이름의 클래스가 있어도 적용이 된다는 점 유의)

# Advice를 정의하는 어노테이션
* @Before("pointcut")
    * 타겟 객체의 메서드가 실행되기 전에 호출되는 어드바이스
    * JoinPoint를 통해 파라미터 정보를 참조할 수 있다
* @After("pointcut") 
    * 타겟 객체의 메서드가 정상 종료됐을 때와 예외가 발생했을 때 모두 호출되는 어드바이스
    * 리턴값이나 예외를 직접 전달받을 수는 없다
* @Around("pointcut") 
    * 타겟객체의 메서드가 호출되는 전 과정을 모두 담을 수 있는 가장 강력한 기능을 가진 어드바이스
* @AfterReturning(pointcut="", returning="")
    * 타겟 객체의 메서드가 정상적으로 실행을 마친 후에 호출되는 어드바이스
    * 리턴값을 참조할 때는 returning 속성에 리턴값을 저장할 변수 이름을 지정해야 한다
* @AfterThrowing(pointcut="", throwing="")
    * 타겟 객체의 메서드가 예외가 발생하면 호출되는 어드바이스
    * 발생된 예외를 참조할 때는 throwing 속성에 발생한 예외를 저장할 변수 이름을 지정해야 한다


# Before 어드바이스
* @Before 어드바이스를 이용해서 실행되는 타겟 객체의 메서드명과 파라미터를 출력하는 어드바이스이다
* 아래의 before 메서드는 myspring 패키지 또는 그 하위 패키지에 있는 모든 public 메서드가 호출되기 이전에 호출된다

    @Before("execution(public * myspring..*(..))")

# AfterThrowing 어드바이스
* @AfterThrowing 어드바이스를 이용해서 실행되는 타겟 객체의 메서드명과 예외 메세지를 출력하는 어드바이스
* 아래의 afterThrowing 메서드는 클래스명이 UserService로 시작되는 클래스에 속한 모든 메서드가 예외가 발생된 이후에 호출된다
* 발생된 예외를 참조할 때는 throwing 속성을 이용해서 예외객체를 담을 변수 이름을 지정해야 한다

# After 어드바이스
* @After 어드바이스를 이용해서 실행되는 타겟 객체의 메서드명을 출력하는 어드바이스
* 아래의 afterFinally 메서드는 메서드명이 User로 끝나는 메서드들이 정상 종료됐을 때와 예외가 발생했을 때 모두 호출된다
* **반드시 반환해야 하는 리소스**가 있거나 메서드 **실행 결과를 항상 로그로 남겨야 하는 경우**에 사용할 수 있다, 하지만 리턴 값이나 예외를 직접 전달받을 수는 없다

    @After("execution(* *..*.*User(..))")