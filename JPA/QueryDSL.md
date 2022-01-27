# QueryDSL
Querydsl은 쿼리를 자바 코드로 작성할 수 있게 해준다. JPA 의 한계였던 동적 쿼리를 해결해주고, 자바 코드이기 때문에 오류 또한 런타임이 아닌 컴파일 시점에 잡아준다. IDE 의 도움도 받을 수 있다.

# Q type
QueryDSL 은 컴파일할 경우 @Entity 와 @Emabdable 등의 어노테이션이 붙은 클래스를 찾아내어 Q 클래스를 만든다.

이 Q 클래스 기반으로 쿼리를 작성하게 된다. 

# 설정
```
@Configuration 
public class QuerydslConfig{
    @PersistenceContext
    private EntityManager em;

    @Bean
    public JPAQueryFactory jpaQueryFactory(){
        return new JPAQueryFactory(entityManager);
    }
}
```

