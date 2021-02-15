# DI(의존성 주입)
각 클래스간의 의존관계를 빈 설정(Bean Definition)(ex. XML, annotation) 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것을 말한다.

* 개발자는 단지 빈 설정파일에서 의존관계가 필요하다는 정보를 추가하면 된다
* 객체 레퍼런스를 컨테이너로부터 주입 받아, 실행 시에 동적으로 의존관계가 생성된다
* 컨테이너가 흐름의 주체가 되어 애플리케이션 코드에 의존관계를 주입해 주는 것이다

# 의존성이란?
함수 또는 클래스가 작동하기 위해서 다른 객체를 필요로 할 때 의존성을 가진다고 한다.

결합도가 높은 상태로써 코드의 재활용성이 떨어지고, 유닛 테스트 작성이 어렵고, 한 객체를 수정했을 때 다른 객체도 수정해줘야하는 문제가 발생할 수 있다

# DI를 했을 때 얻는 이점
1. 코드의 재사용 가능성을 높인다
2. 유닛 테스트 작성이 용이해진다
3. 객체 간의 의존성을 줄여준다
4. 객체 간의 결합도를 낮춰 유연한 코드를 작성할 수 있다 

# DI의 유형
Setter Injection, Constructor Injection, Method Injection으로 나뉜다

## Setter Injection
Setter 메서드를 이용한 의존성 삽입, 의존성을 입력 받는 setter 메서드를 만들고 이를 통해 의존성을 주입한다.

한번에 하나의 의존성밖에 받을 수 없다.

## Constructor Injection 
생성자를 이용한 의존성 삽입, 필요한 의존성을 포함하는 클래스의 생성자를 만들고 이를 통해 의존성을 주입한다.

한번에 여러 개의 의존성을 받을 수 있다.

## Method Injection 
의존성을 입력 받는 일반 메서드를 만들고 이를 통해 의존성을 주입한다.

## DI를 이용한 클래스 호출 방식
![](https://t1.daumcdn.net/cfile/tistory/999E4A3C5C5A75BB27)  
클래스가 구현클래스를 사용한다면 바로 구현클래스를 사용하는 것이 아닌 구현 클래스의 상위 인터페이스를 만들어 놓고 클래스가 인터페이스를 사용하도록 한다. 

구현클래스에 관한 정보는 XML(설정파일)에 기술해 놓는다. 그러면 컨테이너가 설정파일 정보를 읽어 구현 객체를 생성해 주고 클래스에 의존성을 주입해 준다.

# Spring DI 컨테이너의 개념 
Spring DI 컨테이너가 관리하는 객체를 빈(bean)이라고 하고, 이 빈들을 관리한다는 의미로 컨테이너를 빈 팩토리(Bean Factory)라고 부른다.

* 객체의 생성과 객체 사이의 런타임 관계를 DI관점에서 볼 때는 컨테이너를 BeanFactory라고 한다

* Bean Factory에 여러 가지 컨테이너 기능을 추가하여 어플리케이션 컨텍스 라고 부른다

실제로 Spring에는 Bean Factory 인터페이스와 Application Context라는 인터페이스가 있다.

## BeanFactory
* Bean을 등록, 생성, 조회, 반환 관리한다
* 보통은 Bean Factory를 바로 사용하지 않고, 이를 확장한 Application Context를 사용한다.
* getBean() 메서드가 정의되어 있다.

## ApplicationContext
* Bean을 등록, 생성, 조회, 반환 관리하는 기능은 BeanFactory와 같다.
* Spring의 각종 부가 서비스를 추가로 제공한다.
* Spring이 제공하는 Application Context 구현 클래스가 여러 가지 종류가 있다.
* 궁극적으로는 Application Context가 DI 컨테이너 역할을 한다.

# Bean 등록 전략
1. XML 단독 사용
    * 모든 Bean을 명시적으로 XML에 등록하는 방법이다
    * 생성되는 모든 Bean을 XML에서 확인할 수 있다는 장점이 있으나 Bean의 개수가 많아지면 XML파일을 관리하기 번거로울 수 있다
    * 여러 개발자가 같은 설정파일을 공유해서 개발하다 보면 설정파일을 동시에 수정하다가 충돌이 일어나는 경우도 적지 않다
    * DI에 필요한 적절한 setter 메서드 또는 constructor가 코드 내에 반드시 존재해야 한다
    * 개발 중에는 어노테이션 설정방법을 사용했지만, 운영 중에는 관리의 편의성을 위해 XML설정으로 변경하는 전략을 쓸 수도 있다

2. XML과 빈 스캐닝의 혼용
    * Bean으로 사용될 클래스에 특별한 어노테이션을 부여해주면 이런 클래스를 자동으로 찾아서 Bean으로 등록한다
    * 특정 어노테이션이 붙은 클래스를 자동으로 찾아서 Bean으로 등록해주는 방식을 빈 스캐닝을 통한 자동인식 Bean 등록기능이라고 한다
    * 어노테이션을 부여하고 자동 스캔으로 빈을 등록하면 XML문서생성과 관리에 따른 수고를 덜어주고 개발 속도를 향상시킬 수 있다
    * 애플리케이션에 등록될 Bean이 어떤 것들이 있고, Bean들 간의 의존관계가 어떻게 되는지를 한눈에 파악할 수 없다는 단점이 있다


# XML 설정

## property 태그
Setter 메서드를 통해 의존관계가 있는 Bean을 주입하려면 <property> 태그를 사용할 수 있다.

* ref 속성은 사용하면 Bean 이름을 이용해 주입할 Bean을 찾는다
* value 속성은 단순 값 또는 Bean이 아닌 객체를 주입할 때 사용한다

## constructor-arg 태그
Constructor를 통해 의존관계가 있는 Bean을 주입하려면 constructor-arg 태그를 사용할 수 있다.

Constructor 주입방식은 생성자의 피라미터를 이용하기 때문에 한번에 여러 개의 객체를 주입할 수 있다.

    <bean id="hello" class="">
        <constructor-arg index="0" value=""/> //순서대로
        <constructor-arg index="1" ref=""/>  
    </bean>
    <bean id="hello" class="">
        <constructor-arg name="name" value=""/> //변수명으로
        <constructor-arg name="printer" ref=""/>  
    </bean>

## 컬렉션 타입의 값 주입
Spring은 List, Set, Map, Properties와 같은 컬렉션 타입을 XML로 작성해서 property에 주입하는 방법을 제공한다.

List와 Set타입은 list와 value 태그를 사용하면 되고, property가 Set타입이면 list 대신에 set을 사용하면 된다.  

    <property name="">
        <list>
            <value>a</value>
            <value>b</value>
        </list>
    </property>

Map 타입의 경우 map과 entry태그를 이용한다.

    <property name="">
        <map>
            <entry key="" value=""/>
            <entry key="" value=""/>
        </map>
    </property>


## property 파일을 이용한 설정
* XML의 Bean 설정 메타정보는 애플리케이션 구조가 바뀌지 않으면 자주 변경되지 않는다
* property 값으로 제공되는 일부 설정정보는 애플리케이션이 동작하는 환경에 따라서 자주 바뀔 수 있다
* 변경되는 이유와 시점이 다르다면 분리하는 것이 객체지향 설계의 기본 원칙이다. 설정에도 동일한 원칙을 적용할 수 있다
* 환경에 따라 자주 변경될 수 있는 내용은 properties 파일로 분리하는 것이 가장 깔끔하다. XML 처럼 복잡한 구성이 필요 없고 키와 값의 쌍으로 구성하면 된다
    * value 속성에 설정된 값들은 환경에 따라 변경될 수 있는 내용이다
    * 자주 변경되는 값들은 properties 파일에 넣어 분리하는 것이 좋다

## properties 설정 방법
* property 파일로 분리한 정보는 ${}을 이용하여 설정한다.  
* ${} 값을 치환해주는 기능은 context:property-placeholder 태그에 의해 자동으로 등록되는 PropertyPlaceHolderConfigure Bean이 담당한다

## 예시
    <context:property-placeholder location="classpath:"/>
    <bean id="" class="">
        <property name="" value="${}"/>
        <property name="">
            <list>
                <value>${}</value>
            </list>
        </property>
    </bean>

# Annotation 설정

## Bean 등록 Annotation 

### @Component
컴포넌트를 나타내는 일반적인 스테레오 타입으로 bean 태그와 동일한 역할을 함

### @Repository 
persistence 레이어, 영속성을 가지는 속성(파일, 데이터베이스)을 가진 클래스

### @Service
서비스 계층, 비즈니스 로직을 가진 클래스

### @Controller
프레젠테이션 계층, 웹 애플리케이션에서 웹 요청과 응답을 처리하는 클래스


## Bean 의존관계 주입 Annotation

### @Autowired
의존하는 객체를 자동으로 주입해 주는 어노테이션, 타입으로 연결한다.
* @Autowired는 정밀한 의존관계 주입이 필요한 경우에 유용하다
* property, setter 메서드, 생성자, 일반메서드에 적용 가능하다
* 의존하는 객체를 주입할 때 주로 Type을 이용하게 된다
* @Autowired는 property, constructor-arg 태그와 동일한 역할을 한다

### @Resource
의존하는 객체를 자동으로 주입해 주는 어노테이션, 이름으로 연결한다.
* 어플리케이션에서 필요로 하는 자원을 자동 연결할 때 사용된다
* @Resource는 property, setter 메서드에 적용 가능하다
* 의존하는 객체를 주입할 때 주로 Name을 이용하게 된다

### @Value
단순한 값을 주입할 때 사용되는 어노테이션.

### @Qualifier
 * @Autowired 어노테이션과 같이 사용한다
 * @Autowired는 타입으로 찾아서 주입하므로 동일한 타입의 Bean객체가 여러 개 존재할 때 특정 Bean을 찾기 위해서는 @Qualifier을 같이 사용해야 한다


## Component Scan을 지원하는 태그

### context:component-scan 태그
@component를 통해 자동으로 Bean을 등록하고, @Autowired로 의존관계를 주입받는 어노테이션을 클래스에서 선언하여 사용했을 경우에는 해당 클래스가 위치한 특정 패키지를 Scan하기 위한 설정을 XML에 해주어야 한다.

    <context:component-scan base-package=""/>

context:include-filter 태그와 context:exclude-filter 태그를 같이 사용하면 자동 스캔 대상에 포함시킬 클래스와 포함시키지 않을 클래스를 구체적으로 명시할 수 있다.