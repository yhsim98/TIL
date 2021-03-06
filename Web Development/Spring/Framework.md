# Framework

## SW 재사용 방안들
코드를 재사용하는 방법에는 여러가지가 있다.
1. copy & paste
2. Method
3. Class
4. Aop
등이 있는데
1, 2, 3번의 경우 변경이 있을 때 다른 요소에 문제가 생길 수 있다
그래서 개선해서 나온 방법이 AOP이다.

## AOP(Aspect Oriented Programming)
OOP를 서포트 해주는 개념이다.
관심의 분리(Seperatrion of Concerns)라는 개념이다.  
핵심관심모듈, 위빙, 횡단관심모듈로 나뉜다. 핵심관심모듈은 비즈니스 로직, 횡단관심모듈은 기능, 위빙은 둘을 런타임 시점에 합쳐주는 역할을 한다.

## 디자인 패턴
프로그램 개발에서 자주 나타나는 과제를 해결하기 위한 방법 준 하나로 이후 재사용하기 좋은 형태로 특정 규약을 묶어서 정리한 것이다.
대표적으로 GoF(Gang of Four)에서 나오는 23가지 패턴이 있다.

디자인 패턴을 사용하는 이유는 
* 요구사항이 수시로 변경될 수 있다
    * 요구사항 변경에 대한 Source Code 변경을 최소화 해준다
* 프로젝트는 여러 사람이 같이 하는 팀 프로젝트로 진행된다
    * 범용적인 코딩 스타일을 적용애 준다
* 상황에 따라 인수 인계하는 경우도 발생한다
    * 직관적으로 코드를 짤 수 있다

## Framework
**비기능적 요구사항(성능, 보안, 확장성, 안정성 등)을 만족하는 구조**와 구현된 기능을 **안정적으로 실행하도록 제어**해주는 잘 만들어진 구조의 라이브러리 덩어리이다.  

프레임워크는 애플리케이션들의 최소한의 공통점을 찾아 하부 구조를 제공함으로써 개발자들로 하여금 시스템의 하부 구조를 구현하는데 들어가는 노력을 절감하게 해준다.

프레임워크를 사용하는 장점으로는
* 비기능적인 요소들을 초기 개발 단계마다 구현해야 하는 불합리함을 극복해준다
* 기능적인(Functional) 요구사항에 집중할 수 있도록 해준다
* 디자인 패턴과 마찬가지로 반복적으로 발견되는 문제를 해결하기 위한 특화된 Solution을 제공한다.


## 디자인패턴과 프레임워크의 관련성
디자인 패턴은 프레임워크의 핵심적인 특징으로써 프레임워크를 사용하는 어플리케이션에 그 패턴이 적용된다.  

* 디자인 패턴은 어플리케이션을 설계할 때 **구조적인 가이드라인**이 될 수는 있지만 **구현된 기반코드를 제공하지는 않는다.**  
* 프레임워크는 디자인 패턴과 함께 패턴이 적용 된 **기반 클래스 라이브러리를 제공**해서 구조적 틀과 구현 코드를 함께 제공한다.

결론적으로 프레임워크를 사용하면 그 프레임워크의 디자인 패턴이 자동으로 적용된다.

## 프레임워크의 구성요소
프레임워크는 IOC, Class Library, Design pattern으로 구성되어 있다.

## IoC(Inversion of Control)
"제어의 역전"이라는 뜻으로 인스턴스의 생성부터 소멸까지의 **인스턴스 생명주기 관리를 개발자가 아닌 컨테이너가 대신 해준다**는 뜻이다.  
즉, 컨테이너 역할을 해주는 프레임워크에게 제어하는 권한을 넘겨서 개발자의 코드가 신경 써야 할 것을 줄이는 전략이다.

기존에 사용하던 방식으로는  
Library를 사용하는 경우 개발자가 호출하게 되어 개발자가 주도권을 쥐게 된다.
하지만 F/W(Framework)을 사용하면  
F/w이 개발자가 작성한 코드를 호출하게 되어 제어권을 F/W가 가지게 된다.

Spring 컨테이너는 IOC를 지원하며, 메타데이터(XML 설정)를 통해 beans를 관리하고 어플리케이션의 중요부분을 형성한다.

Spring 컨테이너는 관리되는 bean들을 의존성 주입을 통해 IoC를 지원한다.

## 클래스 라이브러리
F/W는 특정 부분의 기술적인 구현을 라이브러리 형태로 제공한다.
라이브러리는 유저의 코드가 호출하는 것이고, F/W는 유저의 코드를 호출한다.


