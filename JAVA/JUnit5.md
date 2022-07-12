# Junit 동작 방식


# JUnit5 컴포넌트
## JUnit Platform
JVM에서 테스트 프레임워크를 실행시키기 위한 기초.

Test Engine API를 제공해 테스트 프레임워크를 개발할 수 있다

## JUnit Vintage
이전 JUnit3과 4를 코드를 위한 테스트 엔진. 필요없으면 제외하면 된다

## JUnit Jupiter
JUnit5에서 테스트를 작성하고 확장하기 위한 새로운 프로그래밍 모델과 확장 모델의 조합

## 요구사항
JUnit5는 java8 부터 지원하며, 이전 버전으로 작성된 테스트 코드여도 컴파일이 정상적으로 지원된다.

# 어노테이션 
여러 어노테이션을 지원한다

대부분 junit-jupiter-api 모듈 안의 ``org.junit.jupiter.api``패키지 안에 존재한다.

## @DisplayName
테스트 클래스나 테스트 메소드에 이름을 붙여줄 때 사용한다.

```
@Test
@DisplayName("테스트1")
void 정책_수정(){

}
```

기본적으로 테스트 이름은 메소드 이름을 따라간다. 하지만 메소드 이름은 그대로 둔 채 테스트 이름을 바꾸고 싶을 때 이 어노테이션을 사용한다.

## @DisplayNameGeneration
클래스 위에 붙이면 테스트 메소드의 이름을 정책에 맞게 변형시켜 줍니다.

``
@DisplayNameGeneration({class<? enxtends DisplayNameGenerator>})
Class TestClass{

}
``

가능한 설정값
* Standard : 기존 클래스, 메소드 명
* Simple : 괄호를 제외
* ReplaceUnderscores : _ 를 공백으로 바꿉니다
* IndicativeSentences : 클래스명 + ","(구분자) + 메소드명으로 바꿉니다
    * @IndicativeSentencesGeneration을 이용하여 구분자를 커스텀할 수 있습니다

``
@DisplayNameGeneration(DisplayNameGenerator.Simple.class)
Class TestClass{

}

@DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscores.class)
Class TestClass{

}

@DisplayNameGeneration(DisplayNameGenerator.IndicativeSentences.class)
Class TestClass{

}

@IndicativeSentencesGeneration(seperator = "->", generator = DisplayNameGenerator.ReplaceUnderscores.class)
Class testClass{

}
``

## @BeforeEach
각각의 테스트 메소드가 실행되기 전 실행되어야 하는 메소드를 명시해준다.

``@Test``, ``@RepeatedTest``, ``@ParameterizedTest``, ``@TestFactory``가 붙은 테스트 메소드가 실행하기 전에 실행된다. JUnit4의 ``@Before``와 같은 역할을 한다.

Mockup 데이터 세팅 등에 사용한다

## @AfterEach
``@Test``, ``@RepeatedTest``, ``@ParameterizedTest``, ``@TestFactory``가 붙은 테스트 메소드가 실행된 후에 실행된다. JUnit4의 ``@After``와 같은 역할을 한다.

## @BeforeAll, @AfterAll
모든 테스트가 실행되기 전 혹은 후 한번만 실행된다.

## @Nested
내부 클래스 테스트 용도

test클래스안에 Nested 테스트 클래스를 작성할 때 사용되며, static이 아닌 중첩 클래스, 즉 Inner 클래스여야만 한다. [테스트 인스턴스 라이프사이클](#테스트-LifeCycle)이 `per-class`로 설정되어 있지 않다면 ```@BeforeAll``, ``@AfterAll``가 동작하지 않으니 주의하자.

## @Tag 
테스트를 필터링할 때 사용한다. 클래스 또는 메소드 레벨에 사용한다

## @Disabled
테스트 클래스나, 메소드의 테스트를 비활성화한다. JUnit4의 ``@Ignore``과 같다

## @Timeout
주어진 시간 안에 테스트가 끝나지 않으면 실패한다

## @ExtendWith
extension을 등록한다. 이 어노테이션은 상속된다. 

## @RegisterExtension
필드를 통해 extension을 등록한다. 

## @TempDir
필드 주입이나 파라미터 주입을 통해 임시적인 디렉토리를 제공할 때 사용한다.

## 메타 어노테이션과 컴포즈 어노테이션
JUnit Jupiter 어노테이션은 메타 어노테이션처럼 사용된다. 

자동으로 메타 어노테이션을 상속하는 자기만의 컴포즈 어노테이션을 정의할 수 있다.

코드에다 ``@Tag("fast")`` 보다는 ``@Fast``를 만든 다음 안에 넣는다

``
@Target()
@Retention
@Tag("fast")
@SpringBootStarter
public @interface Fast{}
``

안에 여러 다른 어노테이션을 넣어 커스텀하여 사용할 수 있다


# 테스트 클래스와 메소드
테스트 클래스란 최상위 클래스, 스태틱 멤버 클래스, @Nested 클래스에 적어도 한개의 **@Test** 어노테이션이 달린 테스트 메소드가 포함되어있는 걸 말한다. 테스트 클래스는 **abstract**이면 안되고, 하나의 생성자가 있어야 한다.

테스트 메소드란 @Test, @TeapetedTest 등등 같은 메타 어노테이션이 붙은 메소드를 의미한다.

라이프사이클 메소드란 @BeforeAll, @AfterAll, @BeforeEach, @AfterEach 같은 메타 어노테이션이 메소드에 붙여진 메소드를 말한다.

테스트 메소드와 라이크사이클 메소드는 테스트할 클래스,상혹한 부모 클래스 또는 인터페이스에 선언된다. 추가로 테스트 메소드와 라이크사이클 메소드는 abstract 선언하면 안되고, 어떠한 값도 리턴되서는 안된다.

참고로 접근제어자는 public

# Assertions
Junit Jupiter는 JUnit4로부터 온 assertion 메소드와 새롭게 자바 8 람다 표현식으로 추가된 메소드들이 있다. 

모든 JUnit Jupiter assertion은 정적 메소드이다.

``assertEquals``, ``assertTimeout`` 등이 있다.

AssertJ, Hamcresst, Truth 등 좋은 써드 파티 라이브러리도 많다.

## AssertTimeoutPreemptively
제공된 executable이나 supplier를 `다른 스레드`에서 실행한다. 때문에 실행된 코드가 ThreadLocal에 의존하는 경우 사이드이펙트가 일어날 수 있다

예로 스프링에서 transactional 테스트를 수행하는 경우가 있다.
* Spring의 테스트는 트랜잭션의 상태를 `ThreadLocal`을 이용하여 현재 상태를 테스트 메소드가 실행하기 전에 저장해둔다.
    * 각 thread는 각자의 threadLocal을 가진다
* assertTimeoutPreemptively는 `다른 스레드`에서 제공된 내용을 수행한다
* 결과적으로 assertTimeoutPreemptively()에 제공된 executable이나 supplier가 `트랜잭션에 참여하는 스프링 컴포넌트를 호출`하게 되면 이 컴포넌트는 `테스트가 끝난 후 롤백되지 않는다.`
    * 롤백할 수 있는 정보가 없나?

# 테스트 실행 조건
JUnit Jupiter 에 있는 ``ExecutionCondition``API는 개발자가 특정 조건에 따라 테스트를 진행할지 여부를 결정한다.

이유를 적고 싶다면 ``disabledReason``속성을 지정하면 된다.



# 테스트 LifeCycle





## reference
https://donghyeon.dev/junit/2021/04/11/JUnit5-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C/
https://goodgid.github.io/How-JUnit-Works/