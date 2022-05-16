# Spring-Test
junit을 확장한 스프링의 테스트 라이브러리

# spring-test 어노테이션
## ``@RunWith(SpringJunit4ClassRunner.class)``
ApplicationContext를 만들고 관리하는 작업을 할 수 있도록 JUnit의 기능을 확장해 준다.

스프링의 핵심 기능인 컨테이너 객체를 생성해 테스트에 사용할 수 있도록 해준다.

Spring-Test는 컨테이너 기술을 사용해 싱글톤으로 관리되는 객체를 사용해 모든 테스트에 사용하게 된다.

## ``@ContextConfiguration(locations="classpath:[xml파일위치])``
* 스프링 빈 설정 파일의 위치를 지정할 수 있습니다. 
* ``@RunWith``어노테이션은 컨테이너를 생성하겠다는 의미로, 어떤 파일을 참조할지 모르는 상태이기 때문에 이 어노테이션을 함께 써줘야 합니다
* default위치는 "src/test/resources"폴더입니다
* "file:Full path"형식으로 써주면 운영 개발에서 사용하는 파일을 불러올 수 있습니다
    * `ContextConfiguration(locations = {"file:src/main/webapp/WEB-INF/spring/a.xml})
    * 해당 폴더에 있는 파일 전부 참조


# Spring Boot Test
스프링부트에서는 테스트를 돕는 두 가지 모듈을 제공합니다.

* `Spring-boot-test`, `Spring-boot-test-autoconfigure`입니다

`Spring-boot-starter-test`를 import 하면 두 모듈이 다 포함됩니다. 또한 JUnit Jupiter, AssertJ, Hamcrest 등과 몇몇 다른 유용한 라이브러리도 함께 들어갑니다.


## Spring-boot-starter-test
* JUnit 5
* Spring Test & Spring Boot Test
* AssertJ
* Harmcrest
* Mockito
* JSONassert
* JsonPath

## Testing Spring Boot Application
의존성 주입의 주요한 이점 중 하나는 유닛 테스트가 쉬워진다는 것입니다.

실제 객체가 아닌 *mock*객체를 사용해서 테스트 할 수도 있습니다.

Spring Boot는 `@SpringBootTest`annotation을 제공합니다. 해당 어노테이션은 `spring-test`의 `@ContextConfiguration`어노테이션 대신 사용할 수 있습니다.

해당 어노테이션은 SpringApplication을 통해 테스트에 사용되는 ``ApplicationContext``을 생성해 줍니다.

기본적으로 ``@SpringBootTest``는 서버를 시작하지 않습니다. 테스트의 세분화를 위해 `@SpringBootTest`의 WebEnvironment 속성을 사용할 수 있습니다.

* MOCK(Default) 
    * Web의 `ApplicationContext`를 불러오고 mock web environment를 제공합니다
    * 내장형 서버들은 해당 어노테이션 사용시 시작되지 않습니다
* RANDOM_PORT
    * 실제 웹 환경을 제공해 줍니다.
    * 내장형 서버가 시작됨
    * `@Transactional`테스트는 일반적으로 각 테스트 메소드가 끝난 후 트랜잭션이 롤백되는데, Random_port, Defined_port 방식에서는 롤백되지 않습니다


## Detecting Web Application Type
Spring MVC가 사용된다면, 일반적인 MVC-based application context가 설정됩니다.

하지만 오직 Spring WebFlux만 사용한다면, 자동으로 감지하여 WebFlux-based application context를 대진 설정합니다.

만약 둘 다 사용된다면, Spring MVC가 우선권을 갖습니다. 만약 reactive web application을 테스트하고 싶다면, `@SpringBootTest(properties="spring.main.web-application-type=reactive")`를 통해 반응형 웹을 테스트할 수 있습니다

## Testing with a mock environment
`@SpringBootTest`는 서버를 시작하지는 않지만, web endpoint 테스트를 위한 mock 환경을 제공합니다.

Spring MVC에서는 `MockMvc`를 사용하거나 `WebTestClient`를 통해 웹 endpoints를 query 할 수 있습니다?












 