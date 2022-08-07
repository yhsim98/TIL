# 자바 클래스 로딩
클래스 로더가 .class 파일을 찾아 JVM의 method 영역에 올리는 것 

JVM은 실행될 때 모든 클래스를 메모리에 올려놓지 않습니다. 그때 마다 필요한 클래스를 메모리에 올려 효율적으로 관리하기 위함입니다.

## 자바 클래스 로딩 시점
* 클래스의 인스턴스가 생성될 때
    * new 를 통한 초기화
* `class.forName()`과 함께 Reflection을 사용하는 경우
* 클래스의 정적 변수가 사용될 때(final로 선언된 상수가 아닐 때)
* 클래스의 정적 메소드가 호출될 때

# 초기화
클래스 초기화는 static 블록과 static 멤버 변수의 값을 할당하는 것을 의미합니다.

클래스가 로드되면 초기화도 바로 수행됩니다.

즉, 클래스의 static 멤버 변수는 클래스 로드 시점에 초기화 됩니다.

## 초기화 순서
1. 정적 블록
2. 정적 변수
3. 생성자

``
Class Single{
    static{
        "정적 블록"
    }

    public static Temp temp = new Temp();

    public Single(){
        "생성자"
    }

    class Temp{
        public Temp(){
            "정적변수"
        }
    }
}
``

## 내부 클래스
* static 내부 클래스의 경우 외부 클래스가 로딩되어도 내부 클래스는 로딩되지 않는다
* static 내부 클래스가 로딩되더라도 외부 클래스는 로딩되지 않는다

## 오직 한번만 클래스가 로딩됨을 보장
JSL에 따르면 JVM에 클래스가 로딩되고 초기화될때는 순차적으로 동작함을 보장합니다.
* 스레드 세이프

멀티 스레드 환경에서 여러 스레드가 클래스를 동시에 로딩하려 시도해도 `오직 한개의 클래스만 로딩`됩니다

# 클래스 로더
![](https://goodgid.github.io/assets/img/java/Java-Class-Loader_1.png)
자바 클래스들은 시작 시 한번에 로드되지 않고, 애플리케이션에서 필요할 때 로드된다.

클래스로더는 JRE의 일부로써 `런타임에 클래스를 동적으로 JVM에 로드하는 역할을 수행`하는 모듈

`.class` 파일에서 바이트코드를 읽어 메모리에 저장하는 시스템 아래 3가지 과정을 수행한다.

* 로딩(Loading) : 클래스를 읽어오는 과정
* 링크(Linking) : 레퍼런스를 읽어오는 과정
* 초기화(Initialization) : static 값들을 초기화하고 변수를 할당하는 과정

## 로딩
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

### 클래스 로더 수행 과정
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
    * 혹시라도 바이트 코드를 수정했을 수 있기 때문에
    * `.class`파일의 형식이 유효한지 확인
* Prepare
    * 클래스 변수(static 변수)와 기본값에 필요한 `메모리 영역`을 준비
* Resolve
    * Optional로 동작하는 부분
    * 심볼릭 메모리 레퍼런스를 메소드 영역에 있는 실제 레퍼런스로 교체
        * {Name} object = new {Name}(); 에서 object 부분을 실제 인스턴스를 가리키게 한다.

### 초기화
static 변수의 값을 할당하는 과정, static 블럭이 수행된다.

## Reference
https://velog.io/@skyepodium/%ED%81%B4%EB%9E%98%EC%8A%A4%EB%8A%94-%EC%96%B8%EC%A0%9C-%EB%A1%9C%EB%94%A9%EB%90%98%EA%B3%A0-%EC%B4%88%EA%B8%B0%ED%99%94%EB%90%98%EB%8A%94%EA%B0%80
https://javarevisited.blogspot.com/2012/07/when-class-loading-initialization-java-example.html#axzz7YvSIe8qK
https://leeyh0216.github.io/posts/singleton/