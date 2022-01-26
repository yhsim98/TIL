# Spring Data Jpa 란?
스프링 프레임워크에서 JPA를 편리하게 사용할 수 있도록 하이버네이트를 추상화한 라이브러리이다.

CRUD를 처리하기 위한 공통 인터페이스를 제공함으로써, 데이터 접근 계층을 개발할 때 구현 클래스 없이 인터페이스만 작성해도 개발을 할 수 있다.

일반적인 CRUD 뿐만 아니라 ```User findByAccount(String account);``` 등 메소드 이름을 분석하여 자동으로 동적으로 JPQL 을 생성하여 실행해 준다.

## 설정 및 예시
```
@Configuration
@EnableJpaRepositories(basePackages="")
public class JpaConfig{

}
```
```
public interface MemberRepository extends JpaRepository<Member, Long>{

}
```

## JpaRepository 인터페이스 계층구조
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKcVAb%2FbtqT7XbZscj%2FCKSFUNj6a64iZ7auAZxVJk%2Fimg.png)

스프링 데이터 모듈 안에 있는 것은 스프링 데이터 프로젝트가 공통으로 사용하는 인터페이스이다. 스프링 테이터 JPA 가 제공하는 JpaRepository 인터페이스는 여기에 추가로 JPA 에 특화된 기능을 제공한다.

## 주요 기본 제공 메소드
* save(S) : 새로운 엔티티는 저장하고 이미 있는 엔티티는 수정한다
    * S 는 식별자와 그 자식타입을 의미한다
    * 식별자에 값이 없다면 새로운 엔티티로 판단, 저장한다
* delete(T) : 엔티티 하나를 삭제한다
* findOne(ID) : 엔티티 하나를 조회한다
* getOne(ID) : 엔티티를 프록시로 조회한다
* findAll(...) : 모든 엔티티를 조회한다. 정렬이나 페이징 조건을 파라미터로 제공할 수 있다

## 쿼리 메소드
메소드 이름만으로 쿼리를 동적으로 생성해 주는 등 여러 기능이 있다.

대표적으로
* 메소드 이름으로 쿼리 생성
* 메소드 이름으로 JPA NamedQuery 호출
* @Query 어노테이션을 사용해서 리포지토리 인터페이스에 쿼리 직접 정의
이 있다.

## 메소드 이름으로 쿼리 생성
이메일과 이름으로 회원을 조회하려면 다음과 같은 메소드를 정의하면 된다.

```
List<Member> findByEmailAndName(String email, String name);
```

메소드 이름을 분석하여 자동으로 JPQL을 생성하고 실행한다.

공식 레퍼런스에 어떤 식으로 메소드 이름을 생성해야 하는지 자세히 설명되어 있다.

엔티티의 필드명에 '_' 가 들어있다면 인식이 안된다..

## JPA NamedQuery
레퍼지토리의 메소드위에 @Query(value="") 를 사용하면 직접 JPQL 로 쿼리를 작성할 수 있다.

nativeQuery=true 를 추가하면 네이티브 sql 로 작성이 가능하다

    @Query(value=".....:name")
    ... .....(@Param("name") String name);

이런 식으로 이름 기반 파라미터 바인딩이 가능하다.

메소드 키워드에 따라 다양한 기능을 제공한다. 자세한건 spring data jpa 공식문서 참조하자

## 벌크성 수정 쿼리
여러 엔티티를 한번에 수정하는 벌크성 수정 쿼리는 ```@Modifiying``` 을 붙이면 된다.

@Modifiying 을 붙이지 않으면 런타임 예외가 발생한다. 

```@Modifiying(clearAutomatically)``` 을 사용하면 자동으로 영속성 컨텍스트를 초기화 해준다.

## 페이징과 정렬
스프링 데이터 JPA 에서는 쿼리 메소드에 페이징과 정렬 기능을 사용할 수 있도록 2가지 파라미터를 제공한다

* org.springframework.data.domain.Sort : 정렬 기능
* org.springframework.data.domain.Pageable : 페이징 기능(내부에 sort 포함)

``` 
Page<Member> 

