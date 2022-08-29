# 실행
자바 애플리케이션은 `java`명령어로 실행할 수 있다

실행 시 JRE를 실행하고 main메소드를 포함하고 있는 클래스를 로딩하고, `main()`메서드를 호출한다.

정확히는 `java`명령어 실행 시 `JVM`이 실행되고, `JVM`은 `클래스로더`를 이용하여 `inital class`를 `create`, `link`, `initalize`하고, main 메서드를 호출한다

각 용어에 대한 설명은 [](https://github.com/yhsim98/TIL/blob/master/JAVA/Class%20Loading.md) 참고바랍니다.

# JVM
자바 가상 머신(Java Virtual Machine), 자바 바이트 코드(.class 파일)을 OS에 특화된 코드로 변환(인터프리터와 JIT 컴파일러)하여 실행한다.

JVM자체는 바이트 코드를 실행하는 표준이자 특정 벤더가 구현한 구현체

Oracle이 JVM 표준 스펙을 정해놓았고, 각 OS 개발사가 OS에 맞도록 JVM을 구현해놓았다

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcxCukT%2FbtqNcFOzFZt%2FkqN6tQ5xwbAAounVQxfEJ1%2Fimg.png)

## JRE
자바 실행 환경(Java Runtime Environment)의 약자로 자바 어플리케이션을 실행할 수 있도록 구성된 배포판

JVM + 라이브러리

## JDK
자바 개발 환경, JRE에 개발에 필요한 툴이 추가된 배포판.

## 자바
프로그래밍 언어, JDK에 들어있는 자바 컴파일러(javac)를 사용하여 바이트코드로 컴파일할 수 있다.

JVM은 OS에 특화된 코드로 변환하기 때문에 플랫폼에 종속적인데 반하여 자바는 바이트코드로 변환되는 대상이기 때문에 플랫폼에 독립적

# JVM 구성
JVM은 5가지 컴포넌트로 구성되어 있다.
* 클래스 로더 시스템
* 메모리
* 실행 엔진
* 네이티브 메소드 인터페이스
    * 다른 언어로 작성된 코드를 실행하기 위해 필요한 컴포넌트
* 네이티브 메소드 라이브러리

해당 컴포넌트들은 클래스나 객체를 읽어 메모리에 올리고 실행하는 과정을 수행

## 클래스 로더
자바 클래스들은 시작 시 한번에 로드되지 않고, 애플리케이션에서 필요할 때 로드된다.

클래스로더는 JRE의 일부로써 `런타임에 클래스를 동적으로 JVM에 로드하는 역할을 수행`하는 모듈

`.class` 파일에서 바이트코드를 읽어 메모리에 저장하는 시스템 아래 3가지 과정을 수행한다.

* 로딩(Loading) : 클래스를 읽어오는 과정
* 링크(Linking) : 레퍼런스를 읽어오는 과정
* 초기화(Initialization) : static 값들을 초기화하고 변수를 할당하는 과정

### 로딩
`.class`파일에서 바이트코드를 읽어 적절한 바이너리 데이터를 만들어 메모리의 메소드 영역에 저장하는 과정

### 링크
레퍼런스를 연결하는 과정

### 초기화
static 변수의 값을 할당하는 과정, static 블럭이 수행된다.    


## 메모리
런타임 데이터 영역(Runtime Data Area)이라고도 부른다.

6가지로 구분된다
* PC(Program Counter) Register
* JVM Stacks
* Native Method Stacks
* Heap
* Method Area
    * 논리적으로는 heap영역에 있지만 아니라고 생각하라나 뭐라나
* Run-time Constant Pool

![](https://i.imgur.com/Mh4DuRB.png)

여기서 단위란 `per-class`같은 것을 의미하는 것으로 `생명 주기`와 `생성 단위`를 의미한다.

JVM 단위에 속하는 `힙과 메서드 영역은 JVM이 시작될 때 생성되고, JVM이 종료될 때 소멸되며, JVM 하나에 힙 하나, 메서드 영역도 하나가 생성`된다.

클래스 단위는 클래스와 생명주기를 같이 하며, 클래스 하나에 하나 생성된다.

스레드 단위에 속하는 PC 레지스터 등은 스레드와 생명주기를 함께한다.

### 스택
쓰레드마다 런타임 스택을 만들고, 그 안에 `스택 프레임(SP)`이라 부르는 메소드 호출을 블럭으로 쌓는다. 

* 메소드가 하나 호출될 때마다 새 프레임이 생성되어 스택에 쌓이고 
* 메서드 호출이 정상 완료되거나 예외가 던져지면
* 프레임은 스택에서 빠지며 소멸된다

쓰레드를 종료하면 런타임 스택도 사라진다

### PC 레지스터
쓰레드마다 쓰레드 내 현재 실행한 스택 프레임을 가리키는 포인터가 생성된다.

* 다음번에 실행할 명령어 정보 저장

### 네이티브 메소드 스택
Native method에 대한 스택을 저장한다.

자바가 아닌 다른 언어로 작성된 네이티브 메서드를 지원하기 위해 사용되는 스택

### 힙
Instance를 저장하는 공유자원

* 메소드 영역
    * 클래스 수준의 정보(클래스 이름, 부모 클래스 이름, 메소드, 변수)를 저장하는 영역, 공유자원. 
    * static으로 선언된 것들도 이 영역에 저장.

* Runtime Constant Pool
    * 클래스나 인터페이스가 만들어질 때 메소드 영역에 생기며 각 클래스나 인터페이스마다 Constant pool table을 가지고 있다

## 실행엔진
.class 파일(바이트코드)을

* 바이트코드
    * 컴파일된 코드
    * JVM이 이해할 수 있다
    * JVM이 어셈블리어로 번역한다

인터프리터, JIT 컴파일러, GC로 이루어져 있다.

### 인터프리터
바이트코드를 한 줄씩 번역하여 실행시킨다.

* 자바는 하이브리드 언어이다
* 자바 컴파일러(javac)가 .java파일을 컴파일하여 바이트코드(.class)로 변환한다
* JVM은 런타임시 바이트코드 파일을 한줄한줄 번역하며 실행한다
    * 인터프리터 언어로서 매번 다시 번역
    * 속도가 느리므로 JIT 등을 통해 속도 개선

### JIT(Just In Time) 컴파일러
인터프리터방식의 느린 속도를 개선하기 위해 도입

* 인터프리터의 효율을 높이기 위해 
* 인터프리터가 반복되는 코드를 발견하면 JIT 컴파일러로 네이티브 코드로 바꾼다
* 그 후 이미 번역한 코드는 컴파일된 네이티브 코드를 사용

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdoANQP%2FbtqM639PEix%2Fu7FZUSbwnlW5sizN1GyBh1%2Fimg.png)
출처: https://catch-me-java.tistory.com/11

### GC
더 이상 참조하지 않는 객체를 모아서 정리


## Reference
https://catch-me-java.tistory.com/11
https://yeti.tistory.com/224
https://serverwizard.tistory.com/162
https://catch-me-java.tistory.com/12
https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%EA%B9%8C%EC%A7%80-1/
https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%EA%B9%8C%EC%A7%80-2/
