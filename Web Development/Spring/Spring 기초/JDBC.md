# 커넥션 풀링
미리 정해진 개수만큼의 DB 커넥션을 Pool에 준비해두고, 애플리케이션이 요청할 때마다 Pool에서 꺼내서 하나씩 할당해주고 다시 돌려받아 Pool에 넣는 식의 기법

* 다중 사용자를 갖는 엔터프라이즈 시스템에서라면 반드시 DB 커넥션 풀링 기능을 지원하는 DataSource를 사용해야 한다
* Spring에서는 DataSource를 공유 가능한 Spring Bean으로 등록해 주어 사용할 수 있도록 해준다

# DataSource 구현 클래스 종류

## 테스트환경을 위한 DataSource
### SimpleDriverDataSource
* Spring이 제공하는 가장 단순한 DataSource 구현 클래스이다.
* getConnection()을 호출할 때마다 매번 DB커넥션을 새로 만들고 따로 pool을 관리하지 않으므로 단순한 테스트용으로만 사용해야 한다

### SingleConnectionDriverDataSource
* 순차적으로 진행되는 통합 테스트에서 사용한다
* 매번 DB커넥션을 생성하지 않기 때문에 비교적 빠르다

## 오픈소스 DataSource
### Apache Commons DBCP
* 가장 유명한 오픈소스 DB 커넥션 pool 라이브러리
* setter 메서드를 제공한다



# JDBC란?
자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API이다.  
JDBC는 모든 자바의 데이터 엑세스 기술의 근간이 된다.  
엔티티 클래스와 어노테이션을 이용하는 최신 ORM 기술도 내부적으로는 DB와의 연동을 위해 JDBC를 이용한다.

# Spring JDBC
JDBC의 장점과 단순성을 그대로 유지하면서 **기존 JDBC의 단점을 극복**할 수 있게 해주고, 간결한 형태의 API사용법을 제공하며, JDBC API에서 지원하지 않는 편리한 기능을 제공한다.

* Spring JDBC는 반복적으로 해야 하는 많은 작업들을 대신 해준다
* Spring JDBC를 사용할 때는 실행할 SQL과 바인딩 할 파라미터를 넘겨 주거나, 쿼리의 실행 결과를 어떤 객체에 넘겨 받을지를 지정하는 것만 하면 된다
* Spring JDBC를 사용하려면 먼저, DB 커넥션을 가져오는 DataSource를 Bean으로 등록해야 한다

# Spring JDBC 기능

* Conection 열기와 닫기
    * Connection과 관련된 모든 작업을 Spring JDBC가 필요한 시점에서 알아서 진행한다
    * 진행 중에 예외가 발생했을 때도 열린 모든 Connection 객체를 닫아준다
* Statement 준비와 닫기
    * SQL 정보가 담긴 Statement 또는 PreparedStatement를 생성하고 필요한 준비 작업을 해주는 것도 Spring JDBC가 한다
    * Statement도 Connection과 마찬가지로 사용이 끝나고 나면 Spring JDBC가 알아서 객체를 닫아준다
* Statement 실행
    * SQL이 담긴 statement를 실행하는 것도 Spring JDBC가 해준다
    * Statement의 실행결과는 다양한 형태로 가져올 수 있다
* ResultSet Loop 처리
    * ResultSet(쿼리를 실행한 결과가 담기는 객체)에 담긴 쿼리 실행 결과가 한 건 이상이면 ResultSet 루프를 만들어서 반복해주는 것도 Spring JDBC가 해주는 작업이다 
* Exception 처리와 반환 
    * JDBC 작업 중 발생하는 모든 예외는 Spring JDBC 예외 변환기가 처리한다
* Transaction 처리
    * transaction과 관련된 모든 작업에 대해서는 신경 쓰지 않아도 된다

# JdbcTemplate 클래스
Spring JDBC가 제공하는 클래스 중 JdbcTemplate은 JDBC의 모든 기능을 최대한 활용할 수 있는 유연성을 제공하는 클래스이다.

JdbcTemplate이 제공하는 기능은 실행, 조회, 배치의 세가지 작업이다.
* 실행 : Insert나 Update같이 DB의 데이터에 변경이 일어나는 쿼리를 수행하는 작업
* 조회 : Select를 이용해 데이터를 조회하는 작업
* 배치 : 여러 개의 쿼리를 한 번에 수행해야 하는 작업

# 생성
JdbcTemplate는 DataSource를 피라미터로 받아서 아래와 같이 생성한다.

    JdbcTemplate template = new JdbcTemplate(dataSource);

DataSource는 보통 Bean으로 등록해서 사용하므로 JdbcTemplate이 필요한 DAO 클래스에서 DataSource Bean을 DI 받아서 JdbcTemplate을 생성할 때 인자로 넘겨주면 된다.

JdbcTemplate은 멀티스레드 환경에서도 안전하게 공유해서 쓸 수 있기 때문에 DAO클래스의 인스턴스 변수에 저장해 두고 사용할 수 있다.

# 메서드
## update()
INSERT, UPDATE, DELETE와 같은 SQL을 실행할 때는 JdbcTemplate의 update() 메서드를 사용한다.

    int update(String sql, [SQL 파라미터])

update() 메서드를 호출할 때는 SQL과 함께 바인딩 할 파라미터는 Object타입 가변인자를 사용할 수 있다.

update() 메서드의 리턴되는 값은 SQL 실행으로 영향을 받은 레코드의 개수를 리턴한다.

## queryForObject()
SELECT SQL을 실행하여 하나의 Row를 가져올 때는 JdbcTemplate의 queryForObject() 메서드를 사용한다.

    <T> T queryForObject(String sql, [SQL 파라미터], RowMapper<T> rm)

* SQL 실행 결과는 여러 개의 칼럼을 가진 하나의 로우
* T는 VO 객체의 타입에 해당된다
* SQL 실행 결과로 돌아온 여러 개의 Column을 가진 한 개의 Row를 RowMapper 콜백을 이용해 VO 객체로 매핑 해준다

## query()
SELECT SQL을 실행하여 여러 개의 Row를 가져올 때는 JdbcTemplate의 query() 메서드를 사용한다.

