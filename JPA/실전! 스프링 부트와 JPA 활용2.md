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