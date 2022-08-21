# ArchUnit
애플리케이션의 아키텍처를 테스트 할 수 있는 오픈 소스 라이브러리로, 패키지, 클래스, 레이어, 슬라이스 간의 의존성을 확인할 수 있는 기능을 제공한다.

순환 참조 등을 파악할 수 있다

아키텍처 테스트 유즈 케이스
* A 라는 패키지가 B (또는 C, D) 패키지에서만 사용 되고 있는지 확인 가능.
    * 반대는 아닌지
* Serivce라는 이름의 클래스들이 *Controller 또는 *Service라는 이름의 클래스에서만 참조하고 있는지 확인.
* Service라는 이름의 클래스들이 ..service.. 라는 패키지에 들어있는지 확인.
    * 다른 모듈에 들어있으면 안됨
    * controller, repository 등
* A라는 애노테이션을 선언한 메소드만 특정 패키지 또는 특정 애노테이션을 가진 클래스를 호출하고 있는지 확인.
* 특정한 스타일의 아키텍처를 따르고 있는지 확인.

## 설치
```
<dependency>
    <groupId>com.tngtech.archunit</groupId>
    <artifactId>archunit-junit5-engine</artifactId>
    <version>0.12.0</version>
    <scope>test</scope>
</dependency>
```

주요 사용법
1. 특정 패키지에 해당하는 클래스를 (바이트코드를 통해) 읽어들이고
2. 확인할 규칙을 정의하고
3. 읽어들인 클래스들이 그 규칙을 잘 따르는지 확인한다.

```
@Test
public void Services_should_only_be_accessed_by_Controllers() {
    JavaClasses importedClasses = new ClassFileImporter().importPackages("com.mycompany.myapp");

    ArchRule myRule = classes()
        .that().resideInAPackage("..service..")
        .should().onlyBeAccessed().byAnyPackage("..controller..", "..service..");

    myRule.check(importedClasses);
}
```

JUnit 5 확장팩 제공
* @AnalyzeClasses: 클래스를 읽어들여서 확인할 패키지 설정
* @ArchTest: 확인할 규칙 정의

## ArchUnit: 클래스 의존성 확인하기
테스트 할 내용
* StudyController는 StudyService와 StudyRepository를 사용할 수 있다.
* Study* 로 시작하는 클래스는 ..study.. 패키지에 있어야 한다.
* StudyRepository는 StudyService와 StudyController를 사용할 수 없다.
