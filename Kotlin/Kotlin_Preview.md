https://www.youtube.com/watch?v=8gseVzeMOzk

## Kotlin
* 자바와 100퍼 호환되는 언어
    * Kotlin 파일은 .class 파일로 컴파일 되고 결국 JVM이 돌릴 수 있음

## null safe


## corutine
?? 뭔소리여

# 어디 가서 코프링 매우 알은 체하기! : 9월 우아한테크세미나
### item2. 자바로 역컴파일하는 습관을 들여라
* 코틀린 숙련도를 향상시키는 가장 좋은 방법 중 하나는 작성한 코드가 자바로 어떻게 표현되는지 확인하는 것
* 역컴파일을 통해 예기치 않은 코드 생성을 방지할 수 있음
* 기존 자바 라이브러리와 프레임워크를 사용하며 문제가 발생할 때 빠르게 확인 가능

### 코틀린 컴파일
* 코틀린 파일은 코틀린 컴파일러가 컴파일 하면 .class(바이트코드 파일) 파일로 바뀜
* 우선 바이트코드로 변환 후 애너테이션 프로세싱 등 작동

### item3. 롬북 대신 데이터 클래스를 사용하라
* 자바에서는 롬북의 @Data를 사용하여 보일러플레이트 코드를 생성한다
* 애너테이션 프로세서는 코틀린 컴파일 이후에 동작하기 떄문에 롬북에서 생성된 자바 코드는 코틀린에서 접근할 수 없음

* 데이터클래스를 사용하면 equals, hashCode 등을 자동으로 생성해 준다
* 주 생성자는 하나 이상의 매개변수가 있어야 하며 모든 매개변수는 val 또는 var
* copy를 적절히 사용하면 데이터 클래스를 불변으로 관리 가능