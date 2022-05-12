# JDBC 드라이버
자바 애플리케이션에서 데이터 베이스에 접근하기 위해서는 JDBC API를 주로 사용하고, JDBC API는 JDBC 드라이버를 거쳐 데이터베이스와 통신을 하게 됩니다.

JDBC 드라이버란 자바 프로그램의 요청을 DBMS가 이해할 수 있는 프로토콜로 변환해 주는 클라이언트 사이드 어댑터 입니다.

각각의 DBMS는 자신에게 알맞은 JDBC 드라이버를 제공하고 있습니다.

## JDBC 실행 과정
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcwF60R%2FbtrpDL5kdvI%2FhQ1GvAJ8TCeme50J4Wwmc1%2Fimg.png)

1. 각 DB에 맞는 JDBC 드라이버를 로드합니다
2. 데이터베이스와 연결하여 Connection 객체를 생성합니다
3. 해당 Connection을 사용하여 쿼리를 실행, 결과를 받아 데이터를 처리합니다
4. 사용한 Connection을 close 합니다

## Connection
* DB와 연결된 객체
* Statement 객체를 생성해 준다
* SQL문을 데이터베이스에 전송하거나, 커밋 롤백하는데 사용된다
* Connection 하나 당 트랜잭션 하나를 관리

## 커넥션 풀(DBCP)
DB와 연결을 통한 Connection 객체를 생성은 비용이 큰 작업이다.

따라서 미리 일정 숫자의 Connection을 생성한 후 Connection Pool에 저장하게 된다.

DB와의 연결이 필요하면 해당 Pool에서 Connection을 가져와 사용하고 작업이 끝나면 반환한다.









