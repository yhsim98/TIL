# Testcontainers
자동으로 컨테이너를 이용하여 테스트 환경을 구축해주고 테스트가 끝난다면 컨테이너를 종료해주는 프레임워크

``멱등성``을 제공하여 보다 Production에 가까운 테스트를 수행할 수 있다

당연히 docker가 설치되어 있어야 한다.
https://www.testcontainers.org/supported_docker_environment/windows/
(못해먹겠다)


## 설치
의존성을 주입하면 된다

```
		<dependency>
			<groupId>org.testcontainers</groupId>
			<artifactId>junit-jupiter</artifactId>
			<version>1.12.4</version>
			<scope>test</scope>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.testcontainers/mysql -->
		<dependency>
			<groupId>org.testcontainers</groupId>
			<artifactId>mysql</artifactId>
			<version>1.12.4</version>
			<scope>test</scope>
		</dependency>
```

위의 junit 모듈을 추가하면 JUnit5의 확장 기능들(어노테이션)을 사용할 수 있고

db에 맞는 모듈을 따로 추가해야 해당 db의 컨테이너를 띄울 수 있다

## 사용법
### @TestContainers
해당 테스트 클래스에서 testContainers를 사용할 수 있게 해준다

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@ExtendWith({TestcontainersExtension.class})
@Inherited
public @interface Testcontainers {
    boolean disabledWithoutDocker() default false;
}

@TestContainers
public class Test{

}

```

### container 생성
객체를 생성하면 자동으로 컨테이너가 생성된다

static 하지 않으면 모든 test 메소드에 대해 컨테이너가 생성된다

static 하게 만들면 test class 당 하나의 컨테이너만 생성된다.
```
    private static MySQLContainer mySQLContainer = new MySQLContainer()
            .withDatabaseName("userServiceTest");
```

메소드간 독립성을 위하여 매 메소드 전 db를 지워주는 것이 좋다
```
    @BeforeEach
    void beforeEach(){
        userRepository.deleteAll();
    }
```

### 생명주기
```
    @BeforeAll
    static void beforeAll(){
        mySQLContainer.start();
    }

    @AfterAll
    static void afterAll(){
        mySQLContainer.stop();
    }

```

### 설정
컨테이너가 어떤 포트에 매핑될지는 알 수 없다

datasource를 해당 properties 처럼 설정해줘야 testcontainers 라이브러리가 자동으로 매핑해준다

```
spring.datasource.url=jdbc:tc:mysql:///test
spring.datasource.driver-class-name=org.testcontainers.jdbc.ContainerDatabaseDriver
```

## 기능들
이미지 이름만 있으면 GenericContainer를 통하여 컨테이너를 만들 수 있다

환경변수 설정이 필요하다

```
static GenericContainer container = new GenericContainer("dockerimageName")
    .withEnv();
```

getMappedPort 등으로 매핑된 포트를 확인 가능하다

이 외에도 네트워크, 명령어 실행, 사용 준비 여부 확인, 로그 확인 등 다양한 기능이 있다

# 컨테이너 정보를 스프링 테스트에서 참조하는 방법
스프링의 configuration을 이용하여 컨테이너에 대한 정보를 스프링에 넣을 수 있다

`@ContextConfiguration`
* 스프링이 제공하는 애노테이션으로, 스프링 테스트 컨텍스트가 사용할 설정 파일 또는 컨텍스트를 커스터마이징할 수 있는 방법을 제공한다.

`ApplicationContextInitializer`
* 스프링 ApplicationContext를 프로그래밍으로 초기화 할 때 사용할 수 있는 콜백 인터페이스로, 특정 프로파일을 활성화 하거나, 프로퍼티 소스를 추가하는 등의 작업을 할 수 있다.

`TestPropertyValues`
* 테스트용 프로퍼티 소스를 정의할 때 사용한다.

`Environment`
* 스프링 핵심 API로, 프로퍼티와 프로파일을 담당한다.

전체 흐름
1. Testcontainer를 사용해서 컨테이너 생성
2. ApplicationContextInitializer를 구현하여 생선된 컨테이너에서 정보를 축출하여 Environment에 넣어준다.
3. @ContextConfiguration을 사용해서 ApplicationContextInitializer 구현체를 등록한다.
4. 테스트 코드에서 Environment, @Value, @ConfigurationProperties 등 다양한 방법으로 해당 프로퍼티를 사용한다.


# docker-compose 사용하기
.yml 파일을 참조하여 컨테이너를 생성할 수 있다

1.12.4 이상 버젼을 사용해야 한다

