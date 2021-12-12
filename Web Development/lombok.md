# lombok 이란?
프로젝트를 개발할때 domain 객체에 getter, setter, toString 등을 만들게 된다. 

만약 개발자가 직접 만든다면 잦은 수정과 필드명 추가 변경이 번거로워 진다.

lombok을 사용하게 되면 getter, setter, toString 등의 코드를 컴파일 시간에 어노테이션을 통해 자동으로 생성해준다. 

# 프로젝트에 lombok 설치
우선 maven이나 gradle에 맞게 lombok 의존성을 주입해 준다.

Intellij는 lombok을 인식하지 못하기 때문에 따로 플러그인을 설치해야 한다.

윈도우 기준으로 ctrl + shift + A을 눌러주고 plugins로 검색하면 플러그인 설치 팝업이 나온다. 

lombok을 검색하여 설치하면 된다.

preferences의 build, execution, deployment의 compiler에 들어가 annotaion processor에 enable annotaion processing에 체크해주면 끝이다.

# lombok 기능
## @Getter, @Setter
클래스 위에 적용시키면 모든 변수에 대해 getter, setter가 생성되고 변수 위에 적용시키면 해당 변수에 대해 생성된다.

## @AllArgsConstructor
모든 변수를 사용하는 생성자를 자동완성 시켜준다.

## @NoArgsConstructor
어떠한 변수도 사용하지 않는 기본 생성자를 자동완성 시켜준다.

## @RequiredArgsConstructor
특정 변수만을 활용하는 생성자를 자동원성 시켜준다.

생성자의 인자로 추가할 변수에 @NonNull을 붙이거나 final로 선언해주면 자동으로 의존성을 주입받을 수 있다.

## @EqualsAndHashCode
클래스에 대한 equals 함수와 hashCode 함수를 자동으로 생성해준다.

만약 특정 변수의 이름이 똑같은 경우 equals한 객체로 판단하고 싶은 경우 

    @EqualsAndHashCode(of={"변수명", "변수명"}))

를 사용하면 된다.

## @ToString
클래스의 변수들을 기반으로 ToString 메소드를 자동으로 완성시켜 준다.

제외하고 싶은 변수가 있다면 @ToString.Exclude를 해당 변수에 사용하면 된다.

만약 상위 클래스에 대해서도 toString을 적용시키고자 한다면 상위 클래스에 @ToString(callSuper=true)를 적용시키면 된다.

## @Log4j2
해당 클래스의 로그 클래스를 자동 완성 시켜준다.

## @Builder
해당 클래스의 객체의 생성에 Builder패턴을 적용시켜 준다. 

클래스 위헤 붙이면 모든 변수에 대해 builder가 적용되고, 특정 변수만 적용시키고자 한다면 생성자를 만들고 그 위에 붙여주면 된다.

## @Data
getter, setter, toStirng, requiredArgsConsturcture, hashcode, clone 등등 여러개가 합쳐진 어노테이션이다. 무거운 편