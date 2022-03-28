https://donghyeon.dev/junit/2021/04/11/JUnit5-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C/

# JUnit5 컴포넌트
## JUnit Platform
JVM에서 테스트 프레임워크를 실행시키기 위한 기초.

Test Engine API를 제공해 테스트 프레임워크를 개발할 수 있다

## JUnit Vintage
이전 JUnit3과 4를 코드를 위한 테스트 엔진. 필요없으면 제외하면 된다

## JUnit Jupiter
JUnit5에서 테스트를 작성하고 확장하기 위한 새로운 프로그래밍 모델과 확장 모델의 조합


## 요구사항
JUnit5는 java8 부터 지원하며, 이전 버전으로 작성된 테스트 코드여도 컴파일이 정상적으로 지원된다.

# 어노테이션 
여러 어노테이션을 지원한다

대부분 junit-jupiter-api 모듈 안의 ``org.junit.jupiter.api``패키지 안에 존재한다.

## @DisplayName
테스트 클래스나 테스트 메소드에 이름을 붙여줄 때 사용한다.

```
@Test
@DisplayName("테스트1")
void 정책_수정(){

}
```

기본적으로 테스트 이름은 메소드 이름을 따라간다. 하지만 메소드 이름은 그대로 둔 채 테스트 이름을 바꾸고 싶을 때 이 어노테이션을 사용한다.

