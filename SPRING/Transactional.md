https://mangkyu.tistory.com/154

# Spring이 제공하는 Transaction 핵심 기술
## 트랜잭션 동기화
트랜잭션 동기화란 트랜잭션을 시작하기 위한 Connection 객체를 특별한 저장소에 보관해두고 필요할 때 꺼내쓸 수 있도록 하는 기술이다.

트랜잭션 동기화 저장소는 작업 쓰레드마다 Connection 객체를 독립적으로 관리하기 때문에, 멀티쓰레드 환경에서도 안전하다.

하지만 JDBC가 아닌 Hibernate 와 같은 기술을 사용한다면 Connection이 아닌 Session 객체를 사용하기 때문에 문제가 생길 수 있다. 이러한 기술 종속적인 문제를 해결하기 위해 Spring은 트랜잭션 관리 부분을 추상화한 기술을 제공하고 있다.

## 트랜잭션 추상화
[](!https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F6eLCk%2Fbtq5gFwQO4x%2FKT2qebNokRvImqLY6iKcK0%2Fimg.png)

