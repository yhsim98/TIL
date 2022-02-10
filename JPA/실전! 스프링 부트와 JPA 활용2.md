//https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-API%EA%B0%9C%EB%B0%9C-%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94

# Entity 를 직접 노출하면 안된다.
entity 로 클라이언트로부터 입력을 받거나 반환하게 된다면 변경에 매우 취약해진다. DTO 를 만들자

# N + 1 문제를 조심하자
lazy type 일 경우 추가로 쿼리가 나갈 수 있다. 

fetch join 하자

# 양방향 연관관계를 조심하자
Entity를 직접 반환하는 경우 Json 라이브러리에서 순환참조로 데드록에 거릴 수 있고, lombok 의 toString() 의 경우에도 순환참조할 수 있다.

toString() 은 제외하거나 직접 정의하는 등 조심하고, Entity를 반환할 때 JsonIgnore 로 꼭 끊어주도록 하자.(물론 Entity 를 직접 반환하지 않는 것이 제일 좋다)

# byteBuddy exception
Lazy 로딩의 경우 프록시 객체를 대신 넣게 되는데 spring jpa 의 경우 byteBuddy 를 사용하여 프록시를 생성한다. 

byteBuddy 예외발생하면 프록시부분의 문제이다. 보통 lazy loading 때문에 발생함. 

# JPA 를 사용하여 DTO 로 직접 조회
원하는 칼럼만 선택하여 조회하며 약간의 성능 최적화가 가능하다 정말 약간. 인덱스 튜닝이나 잘하자. 칼럼이 엄청 많거나 트래픽이 엄청 많으면 고민해보자.

코드 재사용성이 떨어진다.

의존성도 강하다

# 컬렉션 조회 최적화
OneToMany 관계의 경우 엔티티내의 collections 조회할 때 최적화

lazy loading 할 경우 entity N개를 조회한다면 연관되어있는 컬렉션 N 개만큼 쿼리가 추가로 나가야 한다. 1 + N 문제가 발생한다. 

```
List<class> list = select class From Class class;

list.foreach(class->class.getStudents.getName()); // n 번만큼 lazy loading
```

fetch join 을 사용하게 되면 연관된 테이블 조회하여 전부 가져온 다음 알아서 컬렉션에 넣어준다. 연관된 컬렉션까지 전부 조회하는데 쿼리가 한번만 날라간다.

fetch join 으로 OneToMany관계의 컬렉션을 조회하게 된다면 조인의 특성에 따라 중복된 데이터가 생길 수 있다. distinct 사용하자.

```
List<class> result = queryFactory.selectFrom(class)
    .distinct()
    .innerJoin(class.students, student)
    .fetchJoin()
    .fetch();
```

distinct 를 사용하게 되면, 쿼리에도 distinct 를 추가해 주고, jpa 에서도 자체적으로 id 가 같다면 중복을 제거해 준다.


***주의사항*** 

1. 일대다 조회를 하는 순간 페이징이 불가능해진다.
    * 전부 조회해 메모리에서 페이징 하므로 굉장히 위험하다
2. 두 개 이상 컬렉션 패치 조인을 하게 되면 힘들어진다
    * 조인 특성상 데이터가 굉장히 부정확해진다


## 컬렉션 패치 조인시 페이징과 한계돌파
* 컬렉션을 페치 조인하면 페이징이 불가능하다
    * 컬렉션을 페치 조인하면 일대다 조인이 발생하므로 데이터가 예측할 수 없이 증가한다
    * 일다대에서 일(1)을 기준으로 페이징을 하는 것이 목적이다. 그런데 데이터는 다(N)를 기준으로 row 가 생성된다
* Order를 기준으로 페이징 하고 싶은데, 다(N)인 OrderItem을 조인하면 OrderItem이 기준이
되어버린다
* 이 경우 하이버네이트는 경고 로그를 남기고 모든 DB 데이터를 읽어서 메모리에서 페이징을 시도한다
* 최악의 경우 장애로 이어질 수 있다.

한계 돌파
* 먼저 ToOne 관계를 모두 페치조인한다
    * 얘는 상관없다
* 컬렉션은 지연 로딩으로 조회한다
* 페이징을 적용하여 조회한다
    * 당연히 One 인 친구를 기준으로 페이징이다
* 지연 로딩 성능 최적화를 위해 ```spring.jpa.properties.hibernate.default_batch_fetch_size```, ```@BatchSize``` 를 적용한다
    * ```spring.jpa.properties.hibernate.default_batch_fetch_size``` : 글로벌 설정
    * ```@BatchSize```  : 개별 최적화
        * 컬렉션 필드 위에다 적용하면 된다
    * 이 옵션을 사용하면 컬렉션이나, 프록시 객체를 조회할 때 N 개만큼 하나하나 조회하는 것이 아니라 한꺼번에 설정한 size 만큼 IN 쿼리로 조회한다
    * size는 in 내의 개수를 의미한다
    * 조회할 것이 10개인데 size가 1이면 쿼리가 10번 날라간다
    * Entity 에만 적용가능

``` 
List<class> result = queryFactory.selectFrom(class)
    .distinct()
    .innerJoin(class.students, student)
    .fetch();

result.foreach(c->{
        for(s : c.getStudents())
            s.getName()
    }); // lazy 강제 초기화

// 당연히 쿼리가 1 + N + N*N 개 날라갈 것 같지만 1 + 1 + 1 개 날라가게 된다!!
// foreach 로 초기화시키는 부분에서 각 루프마다 쿼리가 각자 날라가는 것이 아닌, IN 으로 한번에 날라감!!!!!
// Class{students}, Student{home}, Home
// class -> students -> home 마치 1번, N번, N*N번 날라갈 것처럼 생겼지만
// 1 + 1 + 1 이렇게 각 1번씩 쿼리가 3번 날라감!!
```

정리
* 쿼리 호출 수가 1 + N -> 1 + 1 로 최적화된다
* 패치 조인보다 DB 데이터 전송량이 최적화된다
    * 각각 조회하므로 중복 데이터가 없음
* 패치 조인 방식과 비교하여 쿼리 호출 수가 약간 증가하지만, DB 데이터 전송량이 감소한다
* 컬렉션 패치 조인은 페이징이 불가능하지만, 이 방법은 페이징이 가능하다

결론
* ToOne 관계는 패치 조인해도 페이징에 영향을 주지 않는다. 따라서 ToOne 관계는 페치조인으로 쿼리 수를 줄이고 나머지는 ```batch_fetch_size```로 최적화한다
* size 는 100 ~ 1000 사이를 선택하는 것이 좋다 
    * 너무 많으면 db와 애플리케이션의 순간 부하가 커짐

## DTO 로 바로 조회시 컬렉션
DTO를 바로 조회 시 DTO의 필드에 컬렉션이 있는 경우 DB에서 데이터를 조회해서 바로 컬렉션에 넣을 수는 없다.

ToOne 관계를 조회 후 컬렉션은 별개의 쿼리로 조회해야 한다.

만약 하나를 조회한다면 그냥 조회 후 컬렉션을 별도로 조회해주면 된다.

하지만 여러 개를 조회한다면 컬렉션도 여러 개이기 때문에 각각의 id 로 조회시 쿼리가 조회 개수만큼 N 번 추가로 나가는 1 + N 문제가 생긴다.

```
List<Dto> result = entityRepository.find(---);

for(var r : result){
    ----.findByForeginId(r.getId());
}
```

result 개수 만큼 쿼리가 나간다.


## 최적화 1, 쿼리 1 + 1 번
```
List<Dto> result = entityRepository.find(---);

List<Long> ids = result.stream()
        .map(o->o.getId())
        .collect(Collectors.toList());

List<JoinTableDto> list = queryFactory.selectFrom(joinTable)
    .innerJoin(Entity.joinTable, joinTable)
    .where(joinTable.id.in(ids)) // in을 사용한다
    .fetch();

Map<Long, List<JoinTableDto>> collect = list.stream()
    .collect(Collectors.groupingBy(j->j.getId()));

result.foreach(o->o.setJoinTables(collect.get(o.getId())));
```

위의 코드를 보면 in 을 사용하고 있다. 

in 을 사용하여 DB 에서 한번에 필요한 데이터를 모두 조회 후, 애플리케이션에서 매핑해주고 있다.

참고로 batch_size 설정을 통한 한번에 조회는 오직 Entity 에만 적용할 수 있다. 

## 최적화 2, 쿼리 1번, 플랫 데이터 최적화
플랫하게 데이터를 가져오도록 최적화해서 조회한다.

그냥 데이터 조인해서 전부 가져온 다음 애플리케이션에서 열심히 직접 매핑해 준다.

* 쿼리는 한번이지만 조인으로 인해 중복 데이터가 추가된다
* 애플리케이션에서 추가 작업을 해줘야 한다
* 페이징이 불가능하다


# 정리
권장 순서
1. 엔티티를 우선 조회
    * 페치조인으로 쿼리 수 최적화
    * 컬렉션 최적화
        * 페이징 필요 -> batch size 로 쵲거화
        * 페이징 필요x -> 페치 조인 사용
2. 엔티티 조회 방식으로 해결이 안되면 DTO 조회 방식 사용
3. DTO 조회 방식으로 해결이 안되면 NativeSQL 이나 JDBCTemplate 사용

```
엔티티 조회 방식은 JPA가 많은 부분을 최적화해주기 때문에 페치 조인이나, @BatchSize 등으로 코드를 거의 수정하지 않고, 옵션만 약간 변경해서, 다양한 성능 최적화를 시도할 수 있다.

반면 DTO를 직접 조회하는 방식은 성능을 최적화 하거나 성능 최적화 방식을 변경할 때 많은 코드를 변경해야 한다.

redis 등으로 캐싱을 할때는 DTO 로 캐싱해야 한다.
```


# OSIV와 성능 최적화
* Open Session In View : 하이버네이트
* Open EntityManager In View : JPA(관례상 OSIV)

## OSIV ON  
[]('https://media.vlpt.us/images/yebali/post/2de7305a-15e2-44d3-a575-012f89c1b534/image.png')

* ``spring.jpa.open-in-view=true`` 기본값

이 기본값으로 해놓으면 애플리케이션이 시작할 때 warning 을 던진다.

OSIV 전략은 트랜잭션 시작처럼 최초 데이터베이스 커넥션 시작 시점부터 API 응답이 끝날 때 까지 영속성 컨텍스트와 데이터베이스 커넥션을 유지한다. 커넥션이 살아있어 View Template 이나 API 컨트롤러단에서 지연 로딩이 가능했던 것이다. 

지연 로딩은 영속성 컨텍스트가 살아있어야 가능하고, 영속성 컨텍스트는 기본적으로 데이터베이스 커넥션을 유지한다. 이것 자체가 큰 장점이다.

그런데 이 전략은 **너무 오랜시간동안 데이터베이스 커넥션 리소스를 사용**하기 때문에, 실시간 트래픽이 중요한 애플리케이션에서는 커넥션이 모자랄 수 있다. 이것은 결국 장애로 이어진다.

예를 들어 컨트롤러에서 외부 API를 호출하면 외부 API 대기 시간 만큼 커넥션 리소스를 반환하지 못하고 유지해야 한다.

## OSIV OFF
[]('https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FemsRZ9%2FbtqTLyKOQQ7%2F5cKM4Ma00A7LYKwhde77Zk%2Fimg.png')

* ``spring.jpa.open-in-view=false`` OSIV 종료

OSIV를 끄면 트랜잭션을 종료할 때 영속성 컨텍스트를 닫고, 데이터베이스 커넥션도 반환한다. 따라서 커넥션 리소스를 낭비하지 않는다.

OSIV를 끄면 모든 지연로딩을 트랜잭션 안에서 처리해야 한다. 따라서 지금까지 작성한 많은 지연 로딩코드를 트랜잭션 안으로 넣어야 하는 단점이 있다. 

# 커맨드와 쿼리 분리
별도의 lazy 초기화용 읽기 전용 QueryService 생성.

보통 비즈니스 로직은 특정 엔티티 몇개를 등록하거나 수정하는 것이므로 성능이 크게 문제가 되지 않는다. 그런데 복잡한 화면을 출력하기 위한 쿼리는 화면에 맞추어 성능을 최적화 하는 것이 중요하다. 하지만 그 복잡성에 비해 핵심 비즈니스에 큰 영향을 주는 것은 아니다.

그래서 크고 복잡한 애플리케이션을 개발한다면, 이 둘의 관심사를 명확하게 분리하는 선택은 유지보수 관점에서 충분히 의미 있다.

