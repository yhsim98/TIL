# Flyway
Flyway는 오픈소스 DB 마이그레이션 tool이다.  
마치 git처럼 개발시 사용하는 Local의 DB의 schema의 변경이력관리, 배포시 사용하는 DB schema와 비교 및 변경사항 자동 적용 등등 여러가지 일들을 해준다.

특정 위치에 있는 SQL 파일들이나 Java classes 들을 마이그레이션 가능하다.  
spring 프로젝트의 경우 resources의 db.migrastion 디렉토리가 기본 경로로 설정되어 있다.  

[flyway 공식문서](https://flywaydb.org/documentation/)

# flyway 명령어 
7가지 명령어를 지원한다
1. Migrate
    * 마이그레이션을 진행한다
2. Info
    * 모든 마이그레이션 상세정보를 출력한다
3. Validate 
    * database에 적용된 마이그레이션 정보의 유효성을 검증한다
4. Baseline
    * flyway로 관리하기 이전에 database가 존재시 해당 database를 flyway baseline으로 설정
5. Repair
    * 메타 데이터 테이블 문제를 해결하기 위해 사용한다
    * 실패한 마이그레이션 항복 제거 가능하다
    * 적용된 마이그레이션의 체크섬을 사용 가능한 마이그레이션의 체크섬으로 재정렬
6. Clean
    * flyway가 관리하는 모든 테이블 드랍한다
    * 실행하는데 주의해야 한다
7. Undo
    * 가장 최근에 적용된 버전 마이그레이션을 취소한다

# File naming
![](http://www.popit.kr/wp-content/uploads/2016/11/SqlMigrationNaming-600x185.png)
flyway의 파일명은 다섯가지로 구성되어 있다.
1. prefix
2. version
3. Seporator
4. description
5. suffix

## prefix 
파일의 이름은 V혹은 R로 시작해야 한다. 그 외의 파일명은 flyway가 인식하지 못한다.

* V의 경우는 버젼 마이그레이션이라는 뜻이다. 
* R의 경우는 반복 마이그레이션이라는 뜻이다.

## version
해당 마이그레이션의 version을 나타낸다. 반복 마이그레시연의 경우 반드시 생략해야 한다.

## Seporator
구분자이다. 언더바 2개로 구성되어 있다.
    V1__explain.sql

## description
해당 마이그레이션 파일의 설명이다

## suffix
확장자 명이다. 기본은 .sql이다.

# 동작방식
Flyway가 연결된 데이터베이스에 자동으로 SCHEMA_VERSION 혹은 flyway_schema_history 이라는 메타데이터 테이블을 생성한다.

Flyway는 사용자가 정의한 sql의 파일명을 자동으로 스캔하여, SCHEMA_VERSION에 버전 정보를 남기는 동시에, 데이터베이스에 변경내용을 적용한다.

# 유효성 검사
checksum 방식으로 유효성 검사를 한다. 스크립트 파일 하나마다 checksum 값이 생성되어 flyway_schema_history에 들어가게 된다. 만약 migrate를 진행할 때 각 스크립트 파일의 checksum 값과 flyway_schema_history의 checksum값이 다른경우 스크립트 파일이 수정됬다는 뜻이며 오류가 발생하게 된다.

# 실행방법
실행방법으로는 총 6가지를 지원한다.
1. Command-line : 콘솔에서 명령을 입력하여 실행하는 방법
2. API : java로 작성된 프로그램내에서 API를 이용하여 실행
3. Maven : Maven에 통합하여 실행
4. Gradle : Gradle에 통합하여 실행
5. Ant : Ant에 통합하여 실행
6. SBT : SBT(Scala 빌드 도구)에 통합하여 실행

# 예제
flyway가 어떤 것인지 느껴보기 위해 직접 사용해 보았다.

두가지 방법으로 실행해 보았다.  
mysql은 버젼 8을 사용했고, flyway 의존성은 당연히 추가했다고 가정한다.
우선 resources.db.migration 에 sql스크립트 파일을 추가해 준다.

1. maven의 plugin으로 migration
pom.xml에 해당 코드를 추가하면 된다.

        <build>
            <plugins>
                <plugin>
                    <groupId>org.flywaydb</groupId>
                    <artifactId>flyway-maven-plugin</artifactId>
                    <version>4.0.3</version>
                    <configuration>
                        <url>jdbc:mysql://localhost:3306/test?serverTimezone=Asia/Seoul</url>
                        <user>db유저명</user>
                        <password>db패스워드</password>
                    </configuration>
                </plugin>
            </plugins>
        </build>

사이드바의 메이븐 항목에서 plugins를 보면 flyway가 추가된것을 확인할 수 있다.  


2. xml파일 
<!-- flyway 설정 -->
    <!-- flyway 설정 -->
    <bean id="flywayConfig" class="org.flywaydb.core.api.configuration.ClassicConfiguration">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- 실행시 flyway migration 동작 -->
    <bean id="flyway" class="org.flywaydb.core.Flyway" init-method="migrate">
        <constructor-arg ref="flywayConfig"/>
    </bean>

위 코드로 spring bean을 추가해주면된다.
tomcat 실행 시 자동으로 migration이 진행된다. 