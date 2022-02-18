# 스프링부트 기능정의
* 단독 실행 가능한 스프링 애클리케이션 생성
* 내장 컨테이너로 톰캣, 제티, 언더토우 중에서 선택가능
* 스타터를 통한 간결한 의존성 구성 지원
* 스프링에 대한 자동구성 제공
* 더이상 xml구성 필요없음
* 제품출시 후 운영에 필요한 다양한 기능(상태점검, 모니터링 등) 제공
    * 헬스체크 등
        * 애플리케이션이 다운됬는지 확인

# 스프링 부트 구성요소
* 빌드도구(그레이들 vs 메이븐)
* 스프링 프레임워크
* 스프링 부트
* 스프링 부트 스타터


# 배포
빌드하면 src/main/java와 src/main/resources만 패키징되어 배포가 된다.

스프링 부트에서 빌드를 하면 (Executable(실행가능한)) JAR 또는 WAR란 말이 자주 나온다.

실행가능하다는 것은 기존에는 서버에 Tomcat는 WAS(web application server)가 설치되어 있고 그 WAS의 특정 위치에 springboot.war라는 배포본을 업로드 하면 Tomcat이 그 파일을 읽어들어 배포하는 방식이었다면, 지금의 배포형태는 개발자가 만든 springboot.JAR와 내장형 컨테이너(Tomcat 등) 과 함께 다시 한번 패키징하는 boot repackagin이 발생하게 된다. 

이렇게 하므로써 바로 실행가능한 상태가 된다.  
콘솔에 

    java springboot.jar

하면 바로 실행된다고 한다.

Docker나 쿠버네티스등의 컨테이너 안에서도 실행가능한 JAR를 배포하고 그안에 자바 빌드팩을 설치하면 작은 단위의 컨테이너 안에서 애플리케이션이 실행 가능하다.

# 스프링부터 1.5 vs 2.x
크게는 스프링 프레임워크 4.3을 쓰느냐 혹은 5를 쓰느냐의 차이이다.

1.5버전은 더 이상 업데이트 되지 않음으로 2.0을 사용하도록 하자.


# 설정
스프링부트는 어노테이션을 기반으로 작동한다.

# 대표적 어노테이션
## @SpringBootApplication
스프링 부트 애플리케이션이 시작되는 곳이다. 이 어노테이션을 기준으로 하향식으로 빈 탐색을 진행하기 때문에 프로젝트 최상단에 위치해야 한다.

내부적으로 
1. @Configuration
2. @EnableAutoConfiguration
3. @ComponentScan
어노테이션들이 적용되어 있다.


## @ComponentScan
자동으로 스캔하여 빈을 ApplicationContext에 적재한다. 

# spring-boot-starter
spring-boot-autoconfigure + spring-boot-dependencies가 합쳐져서 동작을 한다.

spirng-boot-starter을 이용하면 필요한 의존성들과 autoconfigure가 자동으로 주입된다.

spring-boot-starter를 사용하면 버젼을 명시할 필요도 없다. 해당 스타터를 만든 개발자들이 스프링 부트 버젼에 따라 호환성 여부를 확인하여 적절한 버젼이 사용되도록 묶어놓았기 때문이다.

## 자동구성(auto-Configuration)
스프링 부트가 기술흐름에 따라 제공하는 **관례적인 구성을** 자동으로 해준다.

application.properties에 환경변수를 추가함으로써 추가적인 설정을 하면 된다.

따로 추가적인 설정은 configuration 클래스를 생성하여 하면 된다.