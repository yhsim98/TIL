# MyBatis
자바 오브젝트와 SQL문 사이의 자동 Mapping 기능을 지원하는 프레임워크

* SQL을 별도의 파일로 분리해서 관리하게 해주며, 객체-SQL 사이의 파라미터 Mapping 작업을 자동으로 해준다
* MyBatis는 Hibernate나 JPA(Java Persistence Api)처럼 새로운 DB 프로그래밍 패러다임을 익혀야 하는 부담이 없이, 개발자가 익숙한 SQL을 그대로 이용하면서 JDBC 코드 작성의 불편함도 제거해주고, 도메인 객체나 VO 객체를 중심으로 개발이 가능하다는 장점이 있다

# MyBatis 특징
* 쉬운 접근성과 코드의 간결함
    * 가장 간단한 퍼시턴스 프레임워크
    * XML 형태로 서술된 JDBC 코드라고 생각해도 될 만큼 JDBC의 모든 기능을 MyBatis가 대부분 제공한다
    * 복잡한 JDBC코드를 걷어내며 깔끔한 소스코드를 유지할 수 있다
    * 수동적인 파라미터 설정과 쿼리 결과에 대한 맵핑 구문을 제거할 수 있다
* SQL문과 프로그래밍 코드의 분리
    * SQL에 변경이 있을 때마다 자바 코드를 수정하거나 컴파일 하지 않아도 된다
    * SQL 작성과 관리 또한 검토를 DBA와 같은 개발자가 아닌 다른 사람에게 맡길 수 있다
* 다양한 프로그래밍 언어로 구현이 가능하다

# MyBatis를 사용하는 데이터 액세스 계층
![](https://media.vlpt.us/images/changyeonyoo/post/bce0a67f-0493-433f-a728-53cee12c5e51/99BE40445C5D2C5719.png)

# MyBatis의 주요 컴포넌트
![](https://media.vlpt.us/images/changyeonyoo/post/677db703-72b7-40a8-9566-e27c9aa3e935/9919384D5C5D2CA30D.png)

SQLSession Factory Builder, SQLSession Factory, SQLSession이 있다.

MyBatis Config File을 XML파일로 작성해두고, 보라색 부분들만 개발자가 작성하면 된다.

Application에서 SQLSession Factory Builder를 호출하고 SQLSession Factory Buidler가 Config File을 읽고 Factory를 생성해 준다. 개발자가 DB에 insert하거나 Read하는 메서드를 호출하면 SQLSession Factory가 SQLSession를 생성하고 개발자가 작성한 Application 코드에 반환해준다. 

SQLSession은 개발자가 작성한 SQL문을 호출해주는 기능을 해준다고 생각하면 된다.

# MyBatis-Spring의 주요 컴포넌트
![](https://media.vlpt.us/images/changyeonyoo/post/3d9362bb-f4ec-49f0-9cfe-393aa59359d5/9919C9455C5D2DAF09.png)

MyBatis 설정파일(SqlMapConfig.xml) : VO 객체의 정보를 설정한다.

SqlSessionFactory : MyBatis 설정파일을 바탕으로 SqlSessionFactory를 생성한다, Spring Bean으로 등록해야 한다.

SqlSessionTemplate : 핵심적인 역할을 하는 클래스로서 SQL 실행이나 트랜잭션 관리를 실행한다. SqlSession 인터페이스를 구현하며, Thread-safe 하다. Spring Bean으로 등록해야 한다.

Mapping 파일 (user.xml) : SQL문과 OR Mapping을 설정한다.

Spring Bean 설정 파일 (mybatisBeans.xml) : SqlSessionFactoryBean을 Bean 등록할 때 DataSource 정보와 MyBatis Config 파일정보, Mapping 파일의 정보를 함께 설정한다. SqlSessionTemplate을 Bean으로 등록한다.

# Mapper 인터페이스
Mapper 인터페이스는 Mapping 파일에 기재된 SQL을 호출하기 위한 인터페이스

* Mapper 인터페이스는 SQL을 호출하는 프로그램을 Type Safe하게 기술하기 위해 등장했다
* Mapping 파일에 있는 SQL을 자바 인터페이스를 통해 호출할 수 있도록 해준다

# Mapper 인터페이스를 사용하지 않았을 경우
* Mapper 인터페이스를 사용하지 않으면, SQL을 호출하는 프로그램은 SqlSession의 메서드의 아규먼트에 문자열로 **네임스페이스+"."+SQL ID**로 지정해야 한다
* 문자열로 지정하기 때문에 오타에 의한 버그가 숨어있거나, IDE에서 제공하는 code assist를 사용할 수 없다

# Mapper를 사용할 경우
* Session의 인터페이스적인 느낌이다
* UserMapper 인터페이스는 개발자가 작성해야 한다
* 패키지 이름+"."+인터페이스 이름+"."+메서드 이름이 네임스페이스+"."+SQL ID가 되도록 네임스페이스와 SQL의 ID를 설정해야 한다
* 네임스페이스 속성에는 패키지를 포함한 Mapper 인터페이스 이름
* SQL ID에는 매핑하는 메서드 이름을 지정해야 한다

# MapperScannerConfigurer
여러개의 Mapper 인터페이스가 있을 때 사용한다

* MapperFactoryBean을 이용해 Mapper 인터페이스를 등록할 때 Mapper 인터페이스의 개수가 많아지게 되면 일일이 정의하는 데 시간이 많이 걸린다
* Mapper 인터페이스의 수가 많아지면 MapperScannerConfigurer를 이용하여 Mapper 인터페이스의 객체를 한 번에 등록하는 것이 편리하다
* MapperScannerConfigurer를 이용하면 지정한 패키지 아래 모든 인터페이스가 Mapper 인터페이스로 간주되어 Mapper 인터페이스의 객체가 DI 컨테이너에 등록된다

# Mybatis 사용하기

## 1. Datasource 등록
sqlSessionFactory는 dataSource를 필요로 하기 때문에 만약 dataSource bean 등록이 안되있다면 등록해야 한다.

        <!-- DataSource 설정 -->
        <bean class="org.springframework.jdbc.datasource.DriverManagerDataSource" id="dataSource">
	        <property name="driverClassName" value="oracle.jdbc.OracleDriver"/>
	        <property name="url" value="jdbc:oracle:thin:@127.0.0.1:1521:xe"/>
	        <property name="username" value="${db.username}"/>
	        <property name="password" value="${db.password}"/>
        </bean>

mySql을 쓰냐 혹은 oracle을 쓰냐에 따라 약간씩 달라질 수 있다.

## 2. sqlSessionFactory 등록
mybatis 스프링 연동모듈에서, SqlSessionFactoryBean은 SqlSessionFactory를 만들기 위해 사용된다. 다음 설정을 추가하면 된다.

    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <property name="mapperLocations" value="classpath*:sample/config/mappers/**/*.xml" />
    </bean>

mapperLocations 프로퍼티는 mapper에 관련된 xml 파일의 위치를 나열한다. mapper 인터페이스의 구현체의 위치라 생각하는게 편할것 같다.

## 3. mapper 등록
매퍼 인터페이스가 정의되어 있다면 다음처럼 MapperFactoryBean을 사용하여 스프링에 추가하면 된다.

    <bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
        <property name="mapperInterface" value="org.mybatis.spring.sample.mapper.UserMapper" />
        <property name="sqlSessionTemplate" ref="sqlSession" />
    </bean>


## 4. DataSourceTransactionManager 등록
스프링 트랜잭션이 가능하도록, 스프링 설정을 다음과 같이 해준다.

    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="dataSource" />
    </bean>

이때 dataSource는 반드시 sqlSessionFactoryBean을 생성할때 사용된 것과 같아야 한다.

## 5. SqlSessionTemplate 등록
mybatis 스프링 연동모듈의 핵심인 sqlSessionTemplate를 등록한다. 

    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <constructor-arg index="0" ref="sqlSessionFactory" />
    </bean>