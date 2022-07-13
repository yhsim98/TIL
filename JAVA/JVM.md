# JVM
자바 가상 머신(Java Virtual Machine), 자바 바이트 코드(.class 파일)을 OS에 특화된 코드로 변환(인터프리터와 JIT 컴파일러)하여 실행한다.

JVM자체는 바이트 코드를 실행하는 표준이자 특정 벤더가 구현한 구현체

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
`.class`파일에서 바이트코드를 읽어 적절한 바이너리 데이터를 만들어 메모리의 메소드 영역에 저장하는 과정, 3가지 클래스 로더를 제공한다

* Bootstrap class loader
    * JAVA_HOME/lib 에 있는 코어 자바 API 제공, 최상위 우선순위를 자기는 클래스 로더
    * 자바 클래스를 로드하는 것이 아닌, 자바 클래스를 로드할 수 있는 자바 자체의 클래스 로더와 최소한의 자바 클래스만 로드
* Extension class loader
    * bootstrap 클래스로더를 부모로 갖는다
    * JAVA_HOME/lib/ext 폴더 또는 java.ext.dirs 시스템 변수에 해당하는 위치에 있는 클래스를 읽는다
* Application class loader
    * 자바 프로그램 실행 시 지정한 Classpath에 있는 클래스 파일 혹은 Jar에 속한 클래스들을 로드한다
    * 어플리케이션 클래스 경로에서 클래스를 읽는다

클래스를 로드할 때 Bootstrap class loader 부터 로드의 역할을 수행하는데 Application class loader 까지도 클래스 정보를 찾지 못하면 ClassNotFoundException이 발생한다.

메소드 영역에 저장하는 데이터는 다음과 같다
* FQCN
    * 클래스가 속한 패키지명을 모두 포함한 이름
* 클래스, 인터페이스, 열거형
* 메소드, 변수

로딩이 끝나면 `Class<User>`와 같은 클래스 객체를 생성하여 힙 영역에 저장한다.

#### 클래스 로더 수행 과정
1. JVM의 Method Area에 클래스가 로드되어 있는지 확인
2. Method Area에 없다면, 애플리케이션 클래스 로더에 클래스 로드를 요청
3. 애플리케이션 클래스 로더는 확장 클래스 로더에 클래스 로드 요청
4. 확장 클래스 로더는 부트스트랩 클래스 로더에 클래스 로드를 요청
5. 부트스트랩 클래스로더는 부트스트랩 Classpath(JDK, JRE, LIB)에 해당 클래스가 있는지 확인, 없다면 확장 클래스로더가 요청 수행
6. 확장 클래스로더는 확장 Classpath에 해당 클래스가 있는지 확인, 없다면 애플리케이션 클래스로더가 요청을 수행
7. 애플리케이션 클래스로더는 애플리케이션 Classpath에 해당 클래스가 있는지 확인, 없다면 ClassNotFoundException 발생

### 링크
레퍼런스를 연결하는 과정

다음 3가지 절차를 수행한다
* Verify
    * `.class`파일의 형식이 유효한지 확인
* Prepare
    * 클래스 변수(static 변수)와 기본값에 필요한 메모리 영역을 준비
* Resolve
    * Optional로 동작하는 부분, 심볼릭 메모리 레퍼런스를 메소드 영역에 있는 실제 레퍼런스로 교체

### 초기화
static 변수의 값을 할당하는 과정, static 블럭이 수행된다.    


## 메모리
![](https://postfiles.pstatic.net/MjAyMjA3MTNfMzIg/MDAxNjU3NzAzNTIxMDcy.ycIW_VIa7F04pq67B7xo9LRget8NbTjNYt9o6ali-Acg.A86yhddNq4E_-VEK2MJ9TS050WGUnDTf6-2xTBQYZdQg.PNG.2008qwe/%EC%BA%A1%EC%B2%98.PNG?type=w773)

6가지로 구분된다
* 스택
* PC(Program Counter) Register
* JVM Stacks
* Native Method Stacks
* Heap
    * Method Area
    * Run-time Constant Pool

### 스택
쓰레드마다 런타임 스택을 만들고 그 안에 스택 프레임이라 부르는 메소드 호출을 블럭으로 쌓는다. 

쓰레드를 종료하면 런타임 스택도 사라진다

### PC
쓰레드마다 쓰레드 내 현재 실행한 스택 프레임을 가리키는 포인터가 생성된다.

### 네이티브 메소드 스택
Native method에 대한 스택을 저장한다

### 힙
Instance를 저장하는 공유자원

### 메소드 영역
클래스 수준의 정보(클래스 이름, 부모 클래스 이름, 메소드, 변수)를 저장하는 영역, 공유자원. 

static으로 선언된 것들도 이 영역에 저장.

### Runtime Constant Pool
클래스나 인터페이스가 만들어질 때 메소드 영역에 생기며 각 클래스나 인터페이스마다 Constant pool table을 가지고 있다

## 