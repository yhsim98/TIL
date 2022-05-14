# Spring Scheduler 
스프링 스케줄러, 스케줄링 기능을 제공하는 Spring 라이브러리

Xml로 핸들링하는 방법과 annotation으로 핸들링하는 방법이 있다.

## 설정
1. Application Class에 ``@EnableScheduling`` 추가
2. scheduler를 사용할 Class에 ``@Component``, Method에 ``@Scheduled``추가

``@Scheduled``규칙
* Method는 void 타입으로
* Method는 매개변수 사용 불가

# 사용법
## fixedDelay 
