# QueryDSL
Querydsl은 Jpql를 자바 코드로 작성할 수 있게 해준다. JPA 의 한계였던 동적 쿼리를 해결해주고, 자바 코드이기 때문에 오류 또한 런타임이 아닌 컴파일 시점에 잡아준다. IDE 의 도움도 받을 수 있다.

# Q type
QueryDSL 은 컴파일할 경우 @Entity 와 @Emabdable 등의 어노테이션이 붙은 클래스를 찾아내어 Q 클래스를 만든다.

이 Q 클래스 기반으로 쿼리를 작성하게 된다. 

# 설정
## maven
버젼은 스프링부트에서 관리해준다.

메이븐의 경우 Q 클래스를 컴파일이 아닌 generate sources and update folders 를 실행해야 Q 클래스가 생성된다.

```<outputDirectory>target/generated-sources/java</outputDirectory>``` 에서 설정한 위치에 Q 클래스가 생성되는데 target 은 기본이 gitIgnore 라 괜찮지만 그 외의 다른 경로의 경우 깃에 포함되지 않도록 주의해야 한다. 

```
<!-- QueryDSL-->
		<dependency>
			<groupId>com.querydsl</groupId>
			<artifactId>querydsl-apt</artifactId>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>com.querydsl</groupId>
			<artifactId>querydsl-jpa</artifactId>
		</dependency>
```

```
<plugin>
	<groupId>com.mysema.maven</groupId>
		<artifactId>apt-maven-plugin</artifactId>
		<version>1.1.3</version>
		<executions>
			<execution>
				<goals>
					<goal>process</goal>
				</goals>
					<configuration>
						<outputDirectory>target/generated-sources/java</outputDirectory>
						<processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
							<options>
								<querydsl.entityAccessors>true</querydsl.entityAccessors>
							</options>
					</configuration>
			</execution>
		</executions>
</plugin>
```

```
@Configuration 
public class QueryDslConfig{
    @PersistenceContext
    private EntityManager em;

    @Bean
    public JPAQueryFactory jpaQueryFactory(){
        return new JPAQueryFactory(em);
    }
}
```

# QueryDSL 작성
Entitiy repository를 직접 정의하게 되면 spring data jpa 가 제공하는 모든 기본 메소드를 직접 재정의해야 하는 문제가 있다.

queryDSL 용 인터페이스를 만든 후 이름에 Impl 을 붙여 구현하는 클래스를 만들면 자동으로 queryDSL이 자동으로 메서드를 넣어준다.