# Chaos Monkey
* 카오스 엔지니어링 툴
    * 프로덕션 환경, 특히 분선 시스템 환경에서 불확실성을 파악하고 해결 방안을 모색하는데 사용하는 툴

* 네트워크 지연
* 서버 장애
* 디스크 오작동
* 메모리 유수
* ....

카오스 멍키 스프링 부트
* 스프링 부트 애플리케이션에 카오스 멍키를 손쉽게 적용해 볼 수 있는 툴
* 즉, 스프링 부트 애플리케이션을 망가트릴 수 있는 툴?!?!?

## 카오스 멍키 스프링 부트 주요 개념
* 공격 대상
    * ```@RestController```
    * ```@Controller```
    * ```@Service```
    * ```@Repository```
    * ```@Component```
* 공격 유형
    * 응답 지연
    * 예외 발생
    * 애플리케이션 종료
    * 메모리 누수


# 사용법

1. pom.xml 에 의존성 추가
```
		<dependency>
			<groupId>de.codecentric</groupId>
			<artifactId>chaos-monkey-spring-boot</artifactId>
			<version>2.1.1</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
```

2. chaos-monkey라는 profile로 실행  
    * ```spring.profiles.active=chaos-monkey```

3. chaos-monkey 엔드포인트 활성화
    * ```management.endpoint.chaosmonkey.enabled=true```
    * ```management.endpoints.web.exposure.include=health,into,chaosmonkey```

4. 대상 지정
    * ```chaos.monkey.watcher.repository=true``` 등등

## CM4SB 응답 지연
응답 지연 이슈 재현 방법

1. Reposirtory Watcher 활성화
    * ```chaos.monkey.watcher.repository=true```
2. 카오스 멍키 활성화
    * ```http post localhost:8080/actuator/chaosmonkey/status```
3. 카오스 멍키 활성화 확인
    * ```http localhost:8080/actuator/chaosmonkey/watchers```
4. 카오스 멍키 지연 공격 설정
    * ``` http POST localhost:8080/actuator/chaosmonkey/assaults level=3 latencyRangeStart=2000 latencyRangeEnd=5000 latencyActive=true```
5. 테스트
    * JMeter 확인

