# NHN-DDD-Lite@Spring

이미지 올리기 힘들어 그냥 제 블로그 링크 올립니다.  
https://yhsim98.tistory.com/54?category=1059235

CONTENTS

1.  복잡성과 위키
2.  지식 탐구
3.  구현

[https://www.youtube.com/watch?v=TdyOH1xZpT8](https://www.youtube.com/watch?v=TdyOH1xZpT8)

# 복잡성과 위키

-   기술은 항상 발전하는데 중요한게 뭘까? 왜 복잡해질까?
-   애플리케이션이 엄청 복잡하기 때문
    -   도메인 자체에 대한 복잡성
    -   사용하는 기술과 도구에 대한 복잡성
-   위기
    -   일정 driven development ㅋㅋㅋㅋ
    -   빠르고 간단하게 구축 → 오픈하면 새로운 요구사항 → 복잡성 증가 → 가파른 비용 증가, 급격한 생산성 감소 → 위기 → 차세대 개편(개편하면서도 레거시 의존) + 레거시 의존 → 빠르고 간단하게 구축
    -   결국 빠르고 간단하게 맞지 않는 접근법으로 구축하니 문제이다
    -   애플리케이션은 시간이 지날수록 복잡해지고 비용이 급격히 증가한다 → 위기
-   DDD로 이 위기를 극복하자!!

# DDD!!

-   문제가 있고 이해당사자들이 있다
-   문제를 해결하기 위해 애플리케이션을 만드는 것
-   문제와 관련된 도메인 지식들이 있고
-   이런 문제들을 DDD에서는 문제공간이라 한다
-   이런 문제 공간을 DDD에서는 해결 공간으로 만들어야 한다
    -   아주 큰 도메인을 서브 도메인으로 분리한 것
-   이렇게 해결 공간을 뽑아내는 것을 DDD 전략적 패턴이라 한다

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJOvqI%2FbtrPtiDVX8O%2FKWO5tZUgzyGjZ4eqc8ZBmK%2Fimg.png)

-   일반 서브 도메인
    -   대부분의 도메인에서 필요한 서브 도메인
    -   계정, 메일 등등
-   지원 서브 도메인
    -   핵심 서브 도메인을 지원하는 서브 도메인
    -   제품 조회, 배송 서비스 등등
-   핵심 서브 도메인
    -   해결할 도메인 영역 중 가장 핵심이 되고 해결해야할 영역
    -   결제, 쿠폰 먹이고 결제하고 기타 등등 복잡하고 어렵다

-   DDD 전략 패턴은 핵심 도메인에 적용해야 한다
-   핵심 서브 도메인을 Model-Driven 하게 개발하는 방법을 DDD의 전략적 패턴이라 한다

### DDD 전술적 패턴들

[##_Image|kage@HvpLH/btrPtwaWdnr/UIKA23t4Q60xD8TfFirPKk/img.png|CDM|1.3|{"originWidth":1305,"originHeight":710,"style":"alignCenter","width":742,"height":404}_##]

-   원래는 DDD 전술적 패턴 중 일부를 적용하는 것을 DDD-Lite 라 한다
-   본 세션에는 DDD 전술적 패턴 전부를 DDD-Lite라 함

### 지식 탐구: 위키

-   이해당사자들과 얘기하며 도메인을 발전시키고 알아가는 과정을 탐구라고 한다

#### Dooray! 위키 지식 탐구

-   계정 일반 도메인
    -   고객 별 조직/계정 관리
    -   테넌트, 조직, 회원이 존재
-   프로젝트 지원 도메인
    -   프로젝트 진행 단위
    -   하나의 테넌트에 복수 프로젝트가 존재
    -   하나의 프로젝트에는 업무, 드라이브, 위키 등이 포함됨
-   위키 핵심 도메인
    -   프로젝트 지식을 누구나 쉽게 등록/편집할 수 있는 서비스
    -   위키 페이지들의 컨테이너
    -   프로젝트와 1:1 관계 

[##_Image|kage@bIicGT/btrPsB49Tyo/5m8nHFIS8Vfz8C9I6DVIAK/img.png|CDM|1.3|{"originWidth":1354,"originHeight":689,"style":"alignCenter","width":815,"height":415,"caption":"도식화한 내용"}_##]

-   도메인 간의 관계 파악

[##_Image|kage@TRDe1/btrPoxCmEck/d8iMjM3Fr7vVCzCqWeAM6k/img.png|CDM|1.3|{"originWidth":1271,"originHeight":618,"style":"alignCenter","width":778,"height":378}_##]

-   도메인을 서비스별로 경계

[##_Image|kage@oi946/btrPrlVPM3R/rkRnPSOXkXpGO1GhrSUKI0/img.png|CDM|1.3|{"originWidth":1292,"originHeight":664,"style":"alignCenter","width":718,"height":369}_##]

-   엔티티 식별
-   기존 Date Driven 에서는 엔티티의 속성에 집중하게 되었다
-   하지만 DDD-lite 에서 실제 엔티티는 행위, 행동이 중요하고 속성은 그 다음이다
-   도메인의 행위가 도메인을 명확하게 표현할 수 있어야 함
-   중요한 건 행위
-   속성은 행위에 필요하면 생성
-   엔티티간 관계 마저도 행위에 필요하면 맺어주는 것

[##_Image|kage@ze5in/btrPp319WMn/kCxIXiWzUWYZI0oUF1iE80/img.png|CDM|1.3|{"originWidth":1246,"originHeight":623,"style":"alignCenter","width":688,"height":344}_##]

-   핵심 도메인 위키는 페이지들을 가지고 있다
-   위키의 페이지는 이력관리가 필요하다

[##_Image|kage@r2WO6/btrPsBc4Nbx/4fudiMdoxRd5H1UKNlrZxK/img.png|CDM|1.3|{"originWidth":1242,"originHeight":656,"style":"alignCenter","width":746,"height":394}_##]

-   엔티티를 간단하게 뽑아봤다
-   엔티티는 식별성이 있고, 생명주기가 있고, 행위가 우선한다
-   행위와 그 와 연관되는 속성이 존재한다
    -   응집력이 중요

[##_Image|kage@bHFPsv/btrPujvCFMX/bkZ2L9u7tPkCv4MKBvQwC0/img.png|CDM|1.3|{"originWidth":1269,"originHeight":650,"style":"alignCenter","width":704,"height":361}_##]

-   식별성은 존재하지 않지만 도메인의 특정 영역을 서술적으로 추상화시킨 것을 값객체라 한다
-   보통 불변으로 구현하기 때문에 부작용이 없고 값객체 자체에 행위를 추가할 수 있다
    -   때문에 개념을 추상화시키고 캡슐화시키기 좋다
    -   보통 수량, 돈, 주소 등을 값객체로 많이 모델링한다

[##_Image|kage@kVspE/btrPsBYpqvU/rKliWGtMeQt4EjOx0pPp1K/img.png|CDM|1.3|{"originWidth":1270,"originHeight":721,"style":"alignCenter","width":760,"height":431}_##][##_Image|kage@b7oGYJ/btrPrnTFq00/SM0CvIOoUUyKdJWQlE9Z3k/img.png|CDM|1.3|{"originWidth":1247,"originHeight":633,"style":"alignCenter","width":679,"height":345}_##]

-   값객체를 나타내는 냄세가 있다
-   보통 접두어가 같다, 접두어가 같으면 거의 값 객체로 표현가능
-   값객체 안에 값객체가 있어도 되고, 엔티티가 있어도 된다

-   위키 페이지에는 참조파일과 멘션이 있어야 한다

[##_Image|kage@8csTU/btrPtvXrtDA/62SpMiEUlgAjOpCxZrfHak/img.png|CDM|1.3|{"originWidth":1245,"originHeight":676,"style":"alignCenter","width":694,"height":377}_##]

-   새로운 영역이 생겼다. 이 영역을 에그리것이라 한다
-   도메인의 불변식을 지키는 단위이자 데이터 변경의 단위이자 트랜잭션의 단위
    -   분산 환경에서는 락의 단위
-   페이지는 페이지 유저와 페이지 파일과 항상 함께 다닌다
    -   따라서 그룹화해서 페이지 에그리것![](https://blog.kakaocdn.net/dn/bHFTHG/btrPtiDZwWU/v772bG3RDeSzwPRKUaf0q0/img.png)
-   위의 내용을 정제한 것
-   페이지 유저는 페이지와 같이 묶이지만,
-   페이지 파일은 페이지와 별개인 유스케이스가 생겼고, PageFile도 에그리것 루트로 바뀜
    -   에그리것 루트 : 에그리것 루트를 통해서면 에그리것 엔티티에 접근 가능
-   에그리것 경계를 어떻게 잡느냐가 DDD-Lite에서 가장 중요
-   에그리것에서 중요한 것이 에그리것 하나에 repository 하나여야 한다

[##_Image|kage@ca36wS/btrPviiMtfb/KmnBJi9rmfe8NFCbH1eBmk/img.png|CDM|1.3|{"originWidth":1242,"originHeight":577,"style":"alignCenter","width":703,"height":327}_##]

-   페이지 유저가 cascade로 결려있고, 영속성 전이가 되서 Page를 통해서만 변경된다
-   에그리것 루트인 Page에 pageUsers에 변경이 일어나면 자동으로 영속성 전이가 일어나서 C던 U던 일어남

-   위키 페이지 내용 수정 유스케이스 발생
    -   권한체크, 내용 변경 ....

-   행위가 있으면 이것을 set을 여러 번 호출하는 것이 아니라 행위로써 도메인 모델에 표현되야 한다

[##_Image|kage@coMmkM/btrPp3nzhrq/kqU9XlKitMsLLlIqwRRe81/img.png|CDM|1.3|{"originWidth":1249,"originHeight":705,"style":"alignCenter","width":594,"height":335}_##]

-   페이지 변경 내역 추가: 페이지 이력
-   두레이 스트림(알림) 발행
-   검색 변경 발행 
    -   본문이 변경되면 검색도 변경되야 함
-   등등 핵심 도메인으로서 여러 요구사항이 존재함

-   먼저 페이지 내용 변경 이벤트
    -   생산자 - 소비자 모델
        -   페이지가 변경되면 이벤트 발생시키고, 소비자가 잡아서 처리?
    -   트랜잭션 미포함
    -   낮은 결합도
    -   페이지 Entity 외 다른 Entity에 의존하는 것들 처리

[##_Image|kage@BVkme/btrPuIotEsH/i3wfYTQTaN1Gc8rMmKFaZK/img.png|CDM|1.3|{"originWidth":1271,"originHeight":603,"style":"alignCenter","width":626,"height":297}_##]

-   페이지 내용 수정 흐름

[##_Image|kage@boG5gp/btrPrrPfztn/bxVh7J0NuNi8yk54gxalY0/img.png|CDM|1.3|{"originWidth":1268,"originHeight":628,"style":"alignCenter","width":684,"height":339}_##]

-   Application service는 트랜잭션과 보안 등을 관리하는 파사드 패턴
    -   트랜잭션이나 보안 같은 애플리케이션 적인 관심사는 처리하되
    -   절대 도메인 로직은 존재하면 안된다
    -   도메인 로직을 로딩해서 위임한다
    -   그래서 파사드
    -   애플리케이션에 진입하는 changeContent()는 유스케이스의 표현
    -   유스케이스 하나가 애플리케이션 하
    -   pageRepository를 통해 페이지를 조회하고, Page라는 어그리겟 루트로 매소드를 위임

[##_Image|kage@bWEEK2/btrPsNkfFqc/ddkqAoh1xKvaFDNrEEqYYk/img.png|CDM|1.3|{"originWidth":1276,"originHeight":497,"style":"alignCenter","width":693,"height":270}_##]

-   Page에서는 도메인 로직 처리하고 도메인 이벤트를 등록
    -   메시지 큐?
    -   물론 Page는 모르게 추상화되어 있다

[##_Image|kage@UgjuH/btrPp3Vvv2E/eIlaP2pk8OWWBLJtdUjHEk/img.png|CDM|1.3|{"originWidth":1270,"originHeight":699,"style":"alignCenter","width":665,"height":366}_##]

-   이벤트는 EventProcessor가 받아 다시 구독자에게 바인딩
-   각자 구독하는 애플리케이션이 알아서 가져가서 처리
    -   구독은 애플리케이션 서비스의 책임
    -   pub-sub 할때 sub는 관심사별로 최대한 분리하는 것이 좋은 모델
    -   히스토리 변경, 노티 발행, 인덱스 서치 등등

#### DomainEvent 구현 선택

-   Spring-Data
    -   @DomainEvents
    -   @AfterDomainEventPublication
    -   AbstractAggregateRoot
    -   등등
    -   트리거가 있어야 한다
    -   Repository#save 라는 메소드를 호출해야 이벤트가 트리거가 되는 문제
-   직접 구현
    -   ThreadLocal
    -   ApplicationEventPublisher
    -   AOP

### 코드로 한번 보자

-   Data-Driven: PageCommandService

[##_Image|kage@bmtMd5/btrPsPh3Nfr/FBL0plKDnn36oOZJOWpPSk/img.png|CDM|1.3|{"originWidth":1267,"originHeight":681,"style":"alignCenter","width":741,"height":398}_##]

-   Domain Driven

[##_Image|kage@qbkpj/btrPtH4CSdr/sP9HGd4DkYekQn4XyLQekK/img.png|CDM|1.3|{"originWidth":1250,"originHeight":645,"style":"alignCenter","width":761,"height":393}_##]

-   각 서비스 의존성

[##_Image|kage@c5YjfN/btrPtIWJBOr/1cc4C5ZK8RyaJDJv1GWfoK/img.png|CDM|1.3|{"originWidth":1260,"originHeight":619,"style":"alignCenter","width":756,"height":371}_##]

-   Page Data-Driven, Domain-Model

[##_Image|kage@sOIAd/btrPsYFS60Z/Aknjp8OGaKOFsh7yAldK81/img.png|CDM|1.3|{"originWidth":1259,"originHeight":642,"style":"alignCenter","width":721,"height":368}_##]

-   PageHistoryService
    -   이벤트로 호출
    -   트랜잭션이 commit 한 다음 호출되기 때문에 새로운 Transaction 생성
    -   애플리케이션 서비스으로서 도메인 로직이 없어야 하고, 도메인 서비스에 위임하고 있다
    -   도메인 서비스란?
        -   무 상태의 도메인을 처리하는 서비스
        -   엔티티와 에그리것과 같은 속성과 행위를 가지는 것이 아니라 행위만 가지고 있는 것을 도메인 서비스
        -   속성이 없어 무상태??

[##_Image|kage@czJSyS/btrPpYs2DX0/5CaIZaM5Gm6YTXNVYIFwNK/img.png|CDM|1.3|{"originWidth":1257,"originHeight":640,"style":"alignCenter","width":691,"height":352}_##]

-   PageContentChanged라는 도메인 이벤트 그 자체
-   이벤트에 관한 속성과 어떤 이벤트인지 알려주고만 있다
-   참고로 sub 입장에서는 순서를 보장하지 않기 때문에 발생일시를 꼭 넣어줘야 멱등성 있게 처리할 수 있다

[##_Image|kage@u9Iov/btrPtvQJVdF/iI5Boomkn0tprBlFYuMPe1/img.png|CDM|1.3|{"originWidth":1259,"originHeight":692,"style":"alignCenter","width":678,"height":373}_##]

#### Repository와 Spring

[##_Image|kage@AABED/btrPrsgjOMj/uJJgTgNkdS0RPsciTudte1/img.png|CDM|1.3|{"originWidth":1277,"originHeight":601,"style":"alignCenter","width":667,"height":314}_##]

-   Repository는 데이터 모델 영역에 있어야 하고 POJO 여야 한다
-   근데 Trade-Off 생각해봐야 함

### 아키텍처와 모듈

#### 헥사고날 아키텍처 

[##_Image|kage@zlilx/btrPuIovjAr/vk5ZjgkFo6bOn5VeVmmq3K/img.png|CDM|1.3|{"originWidth":1245,"originHeight":715,"style":"alignCenter","width":696,"height":400}_##][##_Image|kage@bMPXSY/btrPuOvr45K/B5zK1ovt9tQt6907WtYRT1/img.png|CDM|1.3|{"originWidth":1256,"originHeight":717,"style":"alignCenter","width":729,"height":416}_##]

-   헥사고날 아키텍처의 모든 의존성 화살표는 밖에서 안으로 모여든다
-   계층형 아키텍처의 단점은 결국 도메인이 인프라에 의존하게 되어있다
    -   도메인 관심사와 기술적 관심사가 섞임

[##_Image|kage@lgYz1/btrPoyOS0AU/oAoVNaIkqF4FfYMyIZW0Gk/img.png|CDM|1.3|{"originWidth":1258,"originHeight":704,"style":"alignCenter","width":631,"height":353}_##]

-   똑같이 의존성 방향이 밖에서 안으로 간다
-   Adapter(pub-sub)을 통해 의존성 방향을 역전시켰고(DIP) 도메인 모델의 순수성을 지킴

#### 모듈

[##_Image|kage@bx4GVl/btrPuPgN8TF/xE412kP3twofBn7TK9aPB0/img.png|CDM|1.3|{"originWidth":1253,"originHeight":694,"style":"alignCenter"}_##]

-   패키지 구조는 이렇게 가는 것을 추천

![](kage@do1pQ8/btrPp38363y/chGxidF33mskcVLwxTeXh1/img.png)

-   계층형 아키텍처는 구시대 유물, 헥사고날 아키텍처로 가자

## 결론

도메인이 먼저다!!!

DDD 쓰자 DDD 앞으로는 모두 DDD 쓸거다