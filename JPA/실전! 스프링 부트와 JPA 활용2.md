# Entity 를 직접 노출하면 안된다.
entity 로 클라이언트로부터 입력을 받거나 반환하게 된다면 변경에 매우 취약해진다. DTO 를 만들자

# N + 1 문제를 조심하자
fetch join 하자?

# 양방향 연관관계를 조심하자
Entity를 직접 반환하는 경우 Json 라이브러리에서 순환참조로 데드록에 거릴 수 있고, lombok 의 toString() 의 경우에도 순환참조할 수 있다.

toString() 은 제외하거나 직접 정의하는 등 조심하고, Entity를 반환할 때 JsonIgnore 로 꼭 끊어주도록 하자.(물론 Entity 를 직접 반환하지 않는 것이 제일 좋다)

# byteBuddy exception
Lazy 로딩의 경우 프록시 객체를 대신 넣게 되는데 spring jpa 의 경우 byteBuddy 를 사용하여 프록시를 생성한다. 

byteBuddy 예외발생하면 프록시부분의 문제이다. 보통 lazy loading 때문에 발생함. 


