# Java Reflection
메소드, 클래스, 인터페이스의 행위를 런타임에 검사하거나 수장하기 위해 사용되는 API 입니다.

런타임에 지금 실행되고 있는 클래스를 가져와 실행할 수 있습니다.

* 자바의 클래스 파일은 바이트 코드로 컴파일되어 static 영역에 위치하게 됩니다. 
* 그로 인해 클래스 이름만 알고 있으면 해당 영역을 탐색하여 클래스에 대한 정보를 가져올 수 있습니다
* 컴파일시 해당 클래스가 없더라도 런타임에 해당 클래스를 로딩하여 메타정보를 가져올 수 있습니다

SDK에 API가 공개되지 않은 경우 Android Studio에서 참조할 수 없어 호출할 수 없지만, 실제로 hidden API가 존재하기 때문에 리플렉션을 이용해서 호출할 수 있습니다.

또 테스트 코드 작성을 위해 private 변수를 변경할 때 리플렉션을 사용할 수 있습니다. 3rd party 라이브러리를 사용하고 이것의 private 변수를 변경하고 싶을 때 리플렉션을 사용하면 라이브러리 코드 변경없이 값을 변경할 수 있습니다.

![](https://media.geeksforgeeks.org/wp-content/uploads/20220110121120/javalang.jpg)

## 메소드들
* `.getAnnotations()` : 어노테이션을 가져올 수 있다
* `.getClass()` : 오브젝트가 속한 클래스의 이름을 얻을 수 있다
* `.getConstructors()` : 오브젝트가 속한 클래스의 public 생성자를 얻을 수 있다
* `.getMethods()` : 오브젝트가 속한 클래스의 공개된 메소드들을 얻을 수 있다
* `.getMethod()` : method명칭과 파라미터 타입을 통해 해당 Method를 찾아 반환
    * `.getMethod("{methodName}", int.class, String.class);`
* `.getDeclaredField(String name);`
    * 접근제어자에 상관없이 name과 일치하는 필드를 찾아준다
* `.getField(String name);`
    * 접근 가능한 경우에만 필드를 찾아준다
* `.invoke({instance}, args1, args2 ...);`
    * reflection으로 찾은 method를 실행

## private한 필드나 메소드 접근
`.setAccessible(true);`를 이용하면 private이어도 접근 가능하다

``
@Test
    public void privateMethodAccessTest() throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
        // given
        Study study = new Study(5);
        Method method = study.getClass().getDeclaredMethod("isFull", int.class);
        method.setAccessible(true);

        // when
        boolean isFull = (boolean) method.invoke(study, 5);
        boolean isNotFull = (boolean)  method.invoke(study, 3);

        // then
        assertTrue(isFull, "인원 다 찼는데 안잡힘");
        assertFalse(isNotFull, "인원 다 안찼는데 잡힘");
    }
``

## 사용예시
* Junit4에서 `@Test`어노테이션이 있는 클래스와 메소드들을 찾아볼 때, `Reflection` 기술을 사용 후 유닛 테스트가 동작할 때 `@Test`어노테이션이 붙은 것을 호출한다
    * Junit은 Annotation Processor을 사용하는데 Annotation Processor은 해당 클래스를 실행시키고 Annotation이 달린 Method를 찾는다. 
        * `Annotation Processor`이란 컴파일 단계에서 annotation을 통해 추가 소스파일을 생성하는 기술
        * query dsl, jpa, lombok 등에서 사용한다
    * 즉, Runtime에서 클래스를 로딩하고 해당 클래스의 instance를 생성한다. 이후 `@Test` 주석이 달린 Method를 찾은 후 해당 Method를 실행 시킨다
* 동적 프록시 생성 등에 사용
* 자동 매핑은 다 내부에서 reflection 사용한다고 합니다
* IDE Class를 탐색하기 위해 Reflection을 주로 사용합니다.
    * 객체를 생성하면 내부 메소드를 자동완성으로 제공하는 것도 reflection을 사용한 기술이라고 합니다 
* lombok 에서도 사용
* Spring DI를 위해 spring container에서도 사용

## 장단점
장점
* 확장성
    * 오프벡트의 완전한 이름을 사용하여 확장성 오브젝트들의 인스턴스를 만들어냄으로써 애플리케이션이 외부, 사용자 정의 클래스들을 사용할 수 있게 된다
* 디버깅과 테스트 도구
    * 클래스의 private 멤버를 검사하기 위해 디버거가 리플렉션의 프로퍼티를 사용할 수 있다

단점
* 성능 오버헤드
    * 리플렉션 연산은 느린 편이다. 따라서 자주 호출되는 성능에 민감한 코드에는 적용하지 않아야 한다
* 내부의 노출
    * 코드가 필드 및 메서드에 액세스하는 것과 같이 비반사 코드에서 불법적인 작업을 수행할 수 있으므로 리플렉션을 사용하면 private예기치 않은 부작용이 발생할 수 있어 코드가 제대로 작동하지 않고 이식성이 손상될 수 있습니다 
    * 반사 코드는 추상화를 깨고 플랫폼 업그레이드에 따라 동작을 변경할 수 있습니다


## 결론
아직 왜 필요한지 잘 모르겠습니다.

정말 필요한 상황을 격어보면 확실히 이해가 될 것 같네요.

알것같기도하고..