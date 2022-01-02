http://www.yes24.com/Product/Goods/19040233

1. SQL 중심적인 개발의 문제점
2. JPA 
3. 영속성 컨텍스트
4. 엔티티 매핑
5. 연관관계


# SQL 중심적인 개발의 문제점

## SQL에 의존적인 개발을 피하기 어렵다
중간에 domain 에 변경이 있으면 관계된 모든 sql을 일일히 수정해야 한다.

번거롭고 특정 sql을 실수로 수정하지 않는 등 실수의 여지가 많다

## 패러다임의 불일치
* 객체와 관계형 데이터베이스의 차이

1. 상속
2. 연관관계
3. 데이터 타입
4. 데이터 식별 방법

등에서 여러가지로 힘들어진다.

객체지향적으로 개발할수록 매핑이 힘들어지는 문제가 생긴다.
그래서 대안으로 나온 것 이 JPA

# JPA(Java-Persistent-Api)
JPA는 ORM 기술이다

ORM(Objective-relational mapping)
* 객체는 객체대로, 관계형 db는 관계형 db대로 설계
* ORM 프레임워크가 중간에서 매핑

JPA는 Java 애플리케이션과 JDBC 사이에서 동작한다. DB에서 JDBC 가 받으면 그것을 JPA가 매핑하고, 애플리케이션에서 JPA로 보내면 JPA가 JDBC에 매핑한다.

## JPA 장점
생산성
* CRUD 쿼리가 이미 존재한다

유지보수
* 기존 필드 변경시 SQL은 자동으로 JPA가 처리

패러다임 불일치 해결
* 상속 등 자동으로 처리해준다

신뢰할 수 있는 엔티티, 계층
* 동일한 트랜잭션에서 조회한 엔티티는 같음을 보장한다
    * user1 == user2 보장


## JPA의 성능 최적화 기능
1. 1차 캐시와 동일성 보장
* 엔티티 매니저 안에서 캐시 기능 지원
* 같은 트랜잭션 안에서는 같은 엔티티를 반환 - 약간의 조회 성능 향상 

2. 트랜잭션을 지원하는 쓰기 지연
* 트랜잭션을 커밋할 때까지 비슷한 SQL을 모은다
* JDBC BATCH SQL 기능을 사용해서 한번에 SQL 전송한다

3. 지연 로딩
    
지연 로딩
* 객체가 실제 사용될 때 로딩

즉시로딩 
* JOIN SQL 로 한번에 연관된 객체까지 미리 조회

## JPQL
JPA 가 제공하는 SQL 을 추상화한 객체 지향 쿼리 언어

SELECT, GROUP BY, HAVING, JOIN 등을 지원한다

엔티티 객체를 대상으로 쿼리

## 엔티티 매니저 팩토리와 엔티티 매니저
EntityManagerFactory 라는 클래스가 요청이 올 때마다 EntityManager을 생성, EntityManager 는 DB 커넥션을 사용하여 DB를 사용한다.

# 영속성 컨텍스트
엔티티를 영구 저장하는 환경이라는 뜻

EntitiyManager.persist(entity) 라고 하면 엔티티를 db가 아닌 영속성 컨택스트라는 곳에 저장한다.

## 엔티티의 생명주기
비영속
* 영속성 컨텍스트와 전혀 관계가 없는 새로운 상태
* 객체를 생성하기만 한 상태

영속
* 영속성 컨텍스트에 관리되는 상태 
* 엔티티메니저에 persist 해서 객체를 넣은 상태
* 바로 db에 날라가는 것이 아님

준영속
* 영속성 컨텍스트에 저장되었다가 분리된 상태 
* detach 

삭제
* 삭제된 상태


## 영속성 컨텍스트의 이점
* 1차 캐시
    * 바로 DB 에 가는 것이 아닌 캐시를 먼저 조회한다
    * 캐시에 없다면 DB 를 조회한 후 캐시에 저장한다
    * 트랜잭션 단위로 작동하고 트랜잭션이 끝나면 영속성 컨텍스트도 종료하기 때문에 사실 별로 도움이 안된다

* 동일성 보장
    * == 비교하면 같다

* 트랜잭션을 지원하는 쓰기 지연
    * insert 문을 모았다가 커밋 시점에 한번에 보낸다
    * batch 옵션이다

* 변경 감지(Dirty checking)
    * 엔티티메니저에 객체를 넣고 나면 객체를 변경하기만 해도 DB까지 자동으로 변경됨

* 지연 로딩


## 플러시
영속성 컨텍스트의 변경내용을 데이터베이스에 반영

영속성 컨텍스트를 비우지 않는다

영속성 컨텍스트의 변경내용을 데이터베이스에 동기화

트랜잭션이라는 작업 단위가 중요 -> 커밋 직전에만 동기화 하면 된다

플러시 발생
* 변경 감지
* 수정된 엔티티 쓰기 지연 SQL 저장소에 등록
* 쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 전송(등록, 수정, 삭제 쿼리)

영속성 컨텍스트를 플러시하는 방법
* em.flush() - 직접 호출
* 트랜잭션 커밋 - 플러시 자동 호출
* JPQL 쿼리 실행 - 플러시 자동 호출


## 준영속
객체를 더 이상 영속성 컨텍스트에서 관리하지 않는 상태.

detach, clear, close 로 준영속 상태로 만들 수 있다.


# 엔티티 매핑

## 객체와 테이블 매핑
@Entity
* JPA 가 관리하는 엔티티
* JPA 를 사용해서 테이블과 매핑할 클래스는 @Entity 필수이다.
* 주의
    * 기본 생성자 필수
    * final 클래스, enum, interface, inner 클래스 사용 X
    * 저장할 필드에 final 사용 X 

@Entity 속성
* name
    * JPA 에서 사용할 엔티티 이름 지정
* Table(name="")
    * 매핑할 테이블 지정



### 데이터베이스 스키마 자동 생성
옵션에 따라 테이블 자동으로 변경 가능하다

entity 를 수정하면 그에 맞춰 테이블을 자동으로 수정해준다.


## 필드와 컬럼 매핑

java 타입과 컬럼 타입
* 기본 자료형은 자동으로 매핑된다
* Enum 타입 등은 
    * @Enumerated(EnumType.STRING) : enum 이름으로 매핑
    * @Enumerated(EnumType.STRING) : enum 순서로 매핑
    * 순서는 바뀔 수 있기 때문에 이름으로 매핑하는 것을 권장한다
* 날짜 타입은 @Temporal(TemporalType.TIMESTAMP), timestamp, date, 등 세가지가 있다
    * java 8 이후로는 Tiemstamp, DateTime 등을 지원하기 때문에 쓸 일이 없다
* @Lob 은 큰 컨텐츠에 사용

@Column(name="", nullable=false/true, unique=true/false)


## 기본키 매핑
직접할당은 @Id, 자동할당은 추가로 @GeneratedValue

@GeneratedVlaue(strategy = GenerationType.{})
* 자동할당 전략을 선택 가능하다
* GenerationType.IDENTITY
    * mySql 의 auto_inclement
* GenerationType.SEQUENCE
    * 
* 여러가지 있다, IDENTITY 권장

@SequenceGenerator
* allocation size 를 이용하여 미리 여러 로를 할당하는 방식으로 insert 문이 여러 번 네트워크를 타지 않도록 할 수 있다
    * 성능최적화
    * 동시성문제도 없다
* 

IDENTITY 의 경우 DB에 insert 되야지만 ID 값을 알 수 있다. 그래서 커밋하는 시점이 아닌 영속성 컨텍스트에 들어가는 즉시 db에 insert 문을 바로 보내도록 한다.

한번에 50개씩 할당하여 사용하는 방식으로 최적화 가능하다 

## 데이터 중심 설계의 문제점
객체를 설계할 때 memberId, orderId 등 관계를 나타내는 id 를 가지고 있는 현재 방식은 객체 설계를 테이블 설계에 맞춘 방식

테이블의 외래키를 객체에 그대로 가져온다

객체 그래프 탐색이 불가능해진다

참조가 없으므로 UML도 잘못된다.


# 연관관계

## 단방향 연관관계
@ManyToOne
* 1 대 n 관계라고 jpa 에 알려주는 어노테이션

@JoinColumn(name="FK")
* 조인해야 하는 컬럼을 가리킴

ex)
Team 
@ManyToOne
@JoinColumn(name="USER_ID")
private Long userId;


## 양방향 연관관계
DB에서는 두 테이블 중 하나에만 FK 가 있으면 양쪽 다 접근 가능하다.

하지만 객체에서는 양쪽 다 서로를 참조해야 양방향으로 접근이 가능하다

    ex)
    Member Long id, Team team
    Team    Long id

    Team 에서 Member로 접근 불가

    -->

    Member Long id, Team team
    Team    Long id, List<Member> members

    양방향 접근 가능

@OneToMany(mappedBy="team")  
* List 에 어노테이션을 붙임으로서 연관관계를 알릴 수 있다


## 연관관계의 주인과 mappedBy
객체의 양방향 관계는 사실 양방향 관계가 아닌 서로 다른 단방향 관계 2개

객체를 양방향으로 참조하려면 단방향 연관관계를 2개 만들어야 한다.

객체에서는 단방향 연관관계가 2개이지만, RDB에서는 양방향 연관관계 하나가 존재한다. 따라서 객체의 어떤 단방향 연관관계가 RDB의 연관관계와 매핑될 것인지 결정해야 한다.

이것을 연관관계의 주인이라 한다.

### 연관관계의 주인(Owner)
* 객체의 두 관계중 하나를 연관관계의 주인으로 지정한다
* 연관관계의 주인만이 외래 키를 관리한다(등록, 수정)
* 주인이 아닌쪽은 읽기만 가능하다
* 주인은 mappedBy 속성을 사용하지 않는다
* 주인이 아니면 mappedBy 속성으로 주인을 지정한다

* 외래 키가 있는 곳을 주인으로 정해라!!
    * 성능이슈나 여러 가지 문제를 해결가능

