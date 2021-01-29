# junit
자바에서 사용하는 단위 테스트 작성을 위한 산업 표준 프레임워크이다.

# 어노테이션

## @Test
테스트를 실행해는 메서드가 된다.
Junit은 각각의 테스트가 서로 영향을 주지 않고 독립적으로 실행됨을 원칙으로 하므로 @Test 마다 객체를 생성한다.

## @Ignore
선언된 메서드는 테스트를 실행하지 않게 된다.

## @Before
선언된 메서드는 @Test 메소드가 실행되기 전에 반드시 실행되어 진다.
@Test 메소드에서 공통으로 사용하는 코드를 @Before 메소드에 선언하여 사용하면 된다.

## @After
선언된 메서드는 @Test 메소드가 실행된 후 실행된다.
테스트때 사용한 데이터를 release하거나 할 때 사용한다. 

## @BeforeClass
@Test 메소드보다 먼저 한번만 수행되어야 할 경우에 사용하면 된다.

## @AfterClass
모든 테스트 메소드가 끝난 후에 수행되어야 할 경우 사용하면 된다.

# assert
 assert는 static 메소드라 static import를 해주거나, 호출할 때 메소드앞에 클래스 명을 붙여야 한다.

## assertEquals(a, b) 
객체 A와 B가 일치함을 확인한다.

## assertArrayEquals(a, b)
배열 A와 B가 일치함을 확인한다.

## assertSame(a, b) 
객체 A와 B가 같은 레퍼런스를 가지고 있는 지 확인한다.

## assertTrue(a)
조건 A가 참인가를 확인한다.

## assertNotNull(a) 
객체 A가 null이 아님을 확인한다.

## 그 외
http://junit.sourceforge.net/javadoc/org/junit/Assert.html

# Spring-test
junit F/W를 확장해서 만든, 스프링 F/W에서 제공하는 통합 테스트 및 스프링 MVC 통합 테스트. Java 코드에 대해서 스프링 컨테이너 생성, 트랜잭션, DB와 연동 관리를 지원하며 웹 관련 Mock 테스트를 위한 객체를 지원한다.

# Spring-test annotation

## @RunWith(SpringJUnit4ClassRunner.class)
@RunWith는 jUnit 프레임워크의 테스트 실행방법을 확장할 때 사용하는 어노테이션이다.  
SpringJUnit4ClassRunner라는 클래스를 지정해주면 jUnit이 테스트를 진행하는 중에 ApplicationContext를 만들고 관리하는 작업을 진행해 준다.  
@RunWith 어노테이션은 각각의 테스트 별로 객체가 생성되더라도 싱글톤의 ApplicationCOntext를 보장한다.

## @ContextConfiguration
스프링 빈 설정 파일의 위치를 지정할 때 사용하는 어노테이션

    @RunWith(SpringJunit4ClassRunner.class)
    @ContextConfiguration(locations = "경로")
    public class TestClass{}

## @Autowired
스프링 DI에서 사용되는 특별한 어노테이션.    
해당 변수에 자동으로 빈을 매핑 해준다.  
스프링 빈 설정 파일을 읽기 위해 굳이 GenericXmlApplicationContext를 사용할 필요가 없다.  

    @Autowired
    ApplicationContext context;



