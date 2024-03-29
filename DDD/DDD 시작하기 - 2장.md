## 네 개의 영역

-   표현, 응용, 도메인, 인프라스트럭처는 아키텍처를 설계할 때 출현하는 전형적인 네 가지 영역이다
-   표현 영역
    -   네 영역 중 표현(또는 UI영역)은 사용자의 요청을 받아 응용 영역에 전달하고, 응용 영역의 처리 결과를 다시 사용자에게 보여주는 역할을 한다
    -   스프링 MVC 프레임워크가 표현 영역을 위한 기술이다, 표현 영역의 사용자는 웹 브라우저 이용자일 수 있고, REST API를 호출하는 외부 시스템일 수 있다
    -   HTTP 요청을 응용 영역이 필요로 하는 형식으로 변환하여 응용 영역에 전달하고 응용 영역의 응답을 HTTP 응답으로 변환하여 전송한다
    -   예를 들어 웹 브라우저가 보낸 HTTP 요청을 응용 영역이 필요로 하는 형식의 객체 타입으로 변환하고,
    -   응용 영역이 리턴한 결과를 JSON 형식으로 변환하여 HTTP 응답으로 웹 브라우저에 전송한다
-   응용 영역
    -   시스템이 사용자에게 제공해야 할 기능을 구현
    -   '주문 등록', '상품 상세 조회'와 같은 기능 구현
    -   응용 영역은 기능을 구현하기 위해 도메인 영역의 도메인 모델을 사용한다
    -   주문 취소 기능을 제공하는 응용 서비스를 예로 살펴보면 다음과 같이 주문 도메인 모델을 사용해서 기능을 구현한다

```
public class CancelOrderService {

    @Transactional
    public void cancelOrder(String orderId) {
        Order order = findOrderById(orderId);
        if (order == null) throw new Exception();
        order.cancel();
    }
    ...
}
```

-   응용 서비스는 로직을 직접 수행하기보다는 도메인 모델에 로직 수행을 위임한다
-   위 코드도 주문 취소 로직을 직접 구현하지 않고 Order 객체에 취소 처리를 위임하고 있다![](https://blog.kakaocdn.net/dn/bjxnGs/btrRANiuxlO/FShDTtnlOeYQRgVavo21PK/img.png)
-   인프라스트럭처
    -   인프라스트럭처 영역은 구현 기술에 대한 것을 다룬다
    -   이 영역은 RDBMS 연동을 처리하고, 메시지 큐에 메시지를 전송하거나 수신하는 기능을 구현하고
    -   몽고 DB나 레디스와의 데이터 연동을 처리한다
    -   이 영역은 SMTP를 이용한 메일 발송 기능을 구현하거나, HTTP 클라이언트를 이용해서 REST API를 호출하는 것도 처리한다
    -   인프라스트럭처 영역은 논리적인 개념을 표현하기보다 실제 구현을 다룬다
-   도메인, 응용, 표현 영역은 구현 기술을 사용한 코드를 직접 만들지 않는다
-   대신 인프라스트럭처 영역에서 제공하는 기능을 사용해서 필요한 기능을 개발한다
-   예를 들어 응용 영역에서 DB에 보관된 데이터가 필요하면 인프라스트럭처 영역의 DB 모듈을 사용하여 데이터를 읽어오고
-   비슷하게 외부에 메일을 발송해야 한다면 인프라스트럭처가 제공하는 SMTP 연동 모듈을 이용해서 메일을 발송한다

## 계층 구조 아키텍처

-   네 영역을 구성할때는 계층 구조를 많이 사용한다
    -   표현 -> 응용 -> 도메인 -> 인프라스트럭처
-   표현과 응용은 도메인 영역을 사용하고, 도메인 영역은 인프라 영역을 사용하므로 계층 구조를 적용하기에 적당해 보인다
-   계층 구조는 그 특성상 상위 계층에서 하위 계층으로의 의존만 존재하고, 하위 계층은 상위 계층에 의존하지 않는다
-   예를 들어 표현은 응용에, 응용은 도메인에 의존하지만, 인프라스트럭처 계층이 도메인에 의존하거나 응용 계층에 의존하지 않는다
-   구현의 편리함을 위해 계층 구조를 유연하게 적용하기도 한다
-   예를 들어 응용은 도메인에 의존하지만 인프라 계층에 의존하기도 한다

[##_Image|kage@cVXaOT/btrRDVfw0xT/EE3bgCL9iGqxYkIeaZO6Hk/img.png|CDM|1.3|{"originWidth":924,"originHeight":692,"style":"alignCenter","width":742,"height":556}_##]

-   이러한 계층 구조를 사용할 때 짚고 넘어가야 할 것은, 표현, 응용, 도메인 계층이
-   상세한 구현 기술을 다루는 인프라 계층에 종속된다는 점이다
-   종속되게 됨으로써 2가지 문제가 발생한다
-   첫번째는 인프라 계층에 종속되게 되면 특정 계층만 테스트하기 쉽지 않다는 것이다
-   인프라가 완벽하게 동작해야만 서비스를 테스트할 수 있다

```
public class CalculateDiscountService {
    private DroolsRuleEngine ruleEngine;

    public CalculateDiscountService() {
        ruleEngine = new DroolsRuleEngine();
    }

    public Money calculateDiscount(OrderLine orderLines, String customerId) {
        Customer customer = findCustomer(cutomerId);

        MutableMoney money = new MutableMoney(0);
        List<?> facts = Arrays.asList(customer, money);
        facts.addAll(orderLines);
        ruleEngine.evaluate("discountCalculation", facts);
        return money.toImmutableMoney();
    }
    ...
}
```

-   두 번째는 구현 방식을 변경하기 어렵다는 점이다
-   위 코드를 보면 Drools가 제공하는 타입을 직접 사용하지 않으므로 Drools 자체에 의존하지 않는다고 생각할 수 있다
-   하지만 discountCalculation 문자열은 Drools의 세션 이름을 의미한다
-   따라서 Drools의 세션 이름을 변경하면 CaculateDiscountService의 코드도 함께 변경해야 한다
-   또 MutableMoney는 룰 적용 결괏값을 보관하기 위해 추가한 타입인데 다른 방식을 사용헀다면 필요 없는 타입이다

-   이처럼 CalculateDiscountService가 겉으로는 인프라의 기술에 직접적인 의존을 하지 않는 것처럼 보여도 실제로는 의존하고 있다
-   이런 상황에서 Drools를 다른 구현 기술로 변경하면 코드의 많은 부분을 고쳐야 한다
-   인프라스트럭처에 의존하면 '테스트 어려움'과 '기능 확장의 어려움'이라는 두 가지 문제가 발생한다는 것을 알게 되었다
-   그렇다면 어떻게 해야 이 두 문제를 해소할 수 있을까?
-   해답은 DIP에 있다

## DIP

-   가격 할인 계산을 하려면 고객 정보를 구하고, 고객 정보와 주문 정보를 이용하여 룰을 실행해야 한다
-   고수준 모듈
    -   고객 정보를 구한다
    -   룰을 이용해서 할인 금액을 구한다
-   저수준 모듈
    -   RDBMS에서 JPA로 구한다
    -   Drools로 룰을 적용한다
-   여기서 CaclulateDiscountService는 고수준 모듈이다
-   고수준 모듈은 의미 있는 단일 기능을 제공하는 모듈로 가격 할인 계산이라는 기능을 구현한다
-   고수준 모듈의 기능을 구현하려면 여러 하위 기능이 필요하다
-   저수준 모듈은 하위 기능을 실제로 구현한 것이다
-   JPA를 이용하여 고객 정보를 읽어오는 모듈과 Drools로 룰을 실행하는 모듈이 저수준 모듈이 된다
-   고수준 모듈이 제대로 동작하려면 저수준 모듈을 사용해야 한다
-   그런데 고수준 모듈이 저수준 모듈을 사용하면 앞서 계층 구조 아키텍처에서 언급했던 두 가지 문제, 즉 구현 변경과 테스트가 어렵다는 문제가 발생한다
-   DIP는 이 문제를 해결하기 위해 저수준 모듈이 고수준 모듈에 의존하도록 바꾼다
-   고수준 모듈을 구현하려면 저수준 모듈을 사용해야 하는데, 반대로 저수준 모듈이 고수준 모듈에 의존하도록 하려면 어떻게 해야 할까?
-   비밀은 추상화한 인터페이스에 있다
-   CalculateDiscountService 입장에서는 룰 적용을 Drools로 했는지 자바로 직접 구현했는지 중요하지 않다
-   고객 정보와 구매 정보에 룰을 적용하여 할인 금액을 구한다 라는 것만 중요할 뿐이다
-   CaclulateDiscountService에는 Drools에 의존하는 코드가 없다. 단지 RuleDiscounter가 룰을 적용한다는 사실만 알뿐이다
-   실제 RuleDiscounter 구현 객체는 생성자를 통해 전달받는다

[##_Image|kage@EMHde/btrRFgDKWlO/0kP7IK0Bag6blHTj4tt8T1/img.png|CDM|1.3|{"originWidth":728,"originHeight":340,"style":"alignCenter"}_##]

-   service는 더 이상 구현 기술인 Drools에 의존하지 않는다
-   **룰을 이용한 할인 금액 계산**을 추상화한 RuleDiscounter 인터페이스에 의존할 뿐이다
-   **룰을 이용한 할인 금액 계산**은 고수준 모듈의 개념이므로 RuleDiscounter 인터페이스는 고수준 모듈에 속한다
-   `DroolsRuleDiscounter`는 고수준의 하위 기능인 `RuleDiscounter`를 구현한 것이므로 저수준 모듈에 속한다
-   DIP를 적용하면 위의 그림처럼 저수준 모듈이 고수준 모듈에 의존하게 된다
    -   상속은 의존의 다른 형태
-   고수준 모듈이 저수준 모듈을 사용하려면 고수준 모듈이 저수준 모듈에 의존해야 하는데,
-   반대로 저수준 모듈이 고수준 모듈에 의존한다고 해서 이를 DIP, 의존 역전 원칙이라 부른다
-   DIP를 적용하면 인프라 영역에 의존할 때 발생했던 두 가지 문제인 구현 교체가 어렵다는 것과 테스트가 어려운 문제를 해소할 수 있다
-   먼저 구현 기술 교체 문제를 보자
-   고수준 모듈은 더 이상 저수준 모듈에 의존하지 않고 구현을 추상화한 인터페이스에 의존한다
-   실제 사용할 저수준 구현 객체는 다음 코드처럼 의존 주입을 이용해서 전달받을 수 있다

```
// 사용할 저수준 객체 생성
RuleDiscounter ruleDiscounter = new DrollsRuleDiscounter();

// 생성자 방식으로 주입
CalculateDiscountService disService = new CalculateDiscountService(ruleDiscounter);
```

-   구현 기술을 변경하더라도 사용할 저수준 구현 객체를 생성하는 코드만 변경하면 된다
-   스프링과 같은 의존 주입을 지원하는 프레임워크를 사용하면 설정 코드를 수정해서 쉽게 구현체를 변경할 수 있다
-   테스트 또한 해당 인터페이스에 Mock 같은 대용 객체를 주입하면 되므로 실제 저수준 객체가 존재하지 않아도 테스트 가능하다

### DIP 주의사항

-   DIP를 잘못 생각하면 단순히 인터페이스와 구현 클래스를 분리하는 정도로 받아들일 수 있다
-   DIP의 핵심은 고수준 모듈이 저수준 모듈에 의존하지 않도록 하기 위함인데 DIP를 적용한 결과 구조만 보고
-   저수준 모듈에서 인터페이스를 추출하는 경우가 있다

[##_Image|kage@RkxO7/btrRDVNuTF7/tgkCoaj26w7B3oKQyGnuO1/img.png|CDM|1.3|{"originWidth":756,"originHeight":382,"style":"alignCenter"}_##]

-   위는 잘못된 구조이다
-   도메인 영역은 구현 기술을 다루는 인프라스트럭처 영역에 의존하고 있다
    -   여전히 고수준 모듈이 저수준 모듈에 의존하는 것이다
-   인터페이스를 고수준 모듈인 도메인 관점이 아니라 룰 엔진이라는 저수준 모듈 관점에서 도출한 것이다
-   DIP를 적용할 때 하위 기능을 추상화한 인터페이스는 고수준 모듈 관점에서 도출해라

### DIP와 아키텍처

-   인프라 영역은 구현 기술을 다루는 저수준 모듈이고, 응용과 도메인 영역은 고수준 모듈이다
-   인프라 계층에 DIP를 적용하면 인프라 영역이 응용과 도메인 영역에 의존(상속)하는 구조가 된다

[##_Image|kage@0FceI/btrREWk6YqA/TOKw4gLJwNdkdHDqcxLh4K/img.png|CDM|1.3|{"originWidth":226,"originHeight":400,"style":"alignCenter"}_##]

-   인프라에 위치한 클래스가 도메인이나 응용 영역에 정의한 인터페이스를 상속받아 구현하는 구조가 되므로
-   도메인과 응용 영역에 대한 영향을 주지 않거나 최소화하며 구현 기술을 변경하는 것이 가능하다

[##_Image|kage@E407E/btrRBLdwMuG/r6tW5F6KIrXDWBKhe4zGA1/img.png|CDM|1.3|{"originWidth":786,"originHeight":504,"style":"alignCenter"}_##]

-   인프라 영역의 EmailNotifier 클래스는 응용 영역의 Notifier 인터페이스를 상속받고 있다
-   주문 시 통지 방식에 SMS를 추가해야 한다는 요구사항이 들어왔을 때 응용 영역의 OrderService는 변경할 필요가 없다
-   두 통지 방식을 함께 제공하는 Notifier 구현 클래스를 인프라 영역에 추가하면 된다
-   비슷하게 mybatis 대신 jpa를 구현 기술로 사용하고 싶다면 JPA를 이용한 OrderRepository 구현 클래스를 인프라 영역에 추가하면 된다

## 도메인 영역의 주요 구성요소

-   앞에서 도메인 영역은 도메인의 핵심 모델을 구현한다고 설명했다
-   도메인 영역의 모델은 도메인의 주요 개념을 표현하며 핵심 로직을 구현한다
-   1장의 엔티티와 밸류 타입은 도메인 영역의 주요 구성요소이다
-   이 두 요소와 함께 도메인 영역을 구성하는 요소는 다음과 같다

-   엔티티
    -   고유의 식별자를 갖는 객체로 자신의 라이프 사이클을 갖는다
    -   주문, 회원, 상품과 같이 도메인의 고유한 개념을 표현한다
    -   도메인 모델의 데이터를 포함하며 해당 데이터와 관련된 기능을 함께 제공한다
-   밸류
    -   고유의 식별자를 갖지 않는 객체로 주로 개념적으로 하나인 값을 표현할 때 사용된다
    -   배송지 주소를 표현하기 위한 '주소'나 구매 금액을 위한 '금액'과 같은 타입이 밸류 타입이다
    -   엔티티의 속성으로 사용할 뿐만 아니라 다른 밸류 타입의 속성으로도 사용할 수 있다
-   애그리거트
    -   연관된 엔티티와 밸류 객체를 개념적으로 하나로 묶은 것
    -   Order 엔티티, OrderLine 밸류, Orderer 밸류 객체를 '주문' 애그리거트로 묶을 수 있다
-   리포지터리
    -   도메인 모델의 영속성을 처리한다
    -   예를 들어 DBMS 테이블에서 엔티티 객체를 로딩하거나 저장하는 기능을 제공한다
-   도메인 서비스
    -   특정 엔티티에 속하지 않은 도메인 로직을 제공한다
    -   '할인 금액 계산'은 상품, 쿠폰, 회원 등급, 구매 금액 등 다양한 조건을 이용해서 구현하는데,
    -   이렇게 도메인 로직이 여러 엔티티와 밸류를 필요로 하면 도메인 서비스에서 로직을 구현한다

#### 엔티티와 밸류

-   도메인 모델의 엔티티와 DB 모델의 엔티티는 같지 않다
-   도메인 모델의 엔티티는 데이터와 함께 도메인 기능을 함께 제공한다
    -   예를 들어 주문 엔티티는 주문과 관련된 데이터 뿐만 아니라 배송지 주소 변경을 위한 기능 등등
-   도메인 모델의 엔티티는 단순히 데이터를 담는 데이터 구조를 넘어 기능을 함께 제공하는 객체이다
-   도메인 관점에서 기능을 구현하고 기능 구현을 캡슐화하여 데이터가 임의로 변경되는 것을 막는다

-   또 다른 차이점은 도메인 모델의 엔티티는 두 개 이상의 데이터가 개념적으로 하나인 경우 밸류 타입을 이용해서 표현할 수 있다는 것이다

```
class Order {
	...
    Private Orderer orderer;
    ...
}

class Orderer {
	private String name;
    private String email;
    ...
}
```

-   RDBMS와 같은 관계형 데이터베이스는 밸류 타입을 제대로 표현하기 힘들다
-   데이터를 개별로 저장하거나 별도 테이블로 분리해야 한다
    -   개별로 저장하면 데이터가 같은 개념에 속한다는 것을 드러낼 수 없다
    -   별도 테이블로 저장하면 엔티티에 가까워지며 벨류 타입의 의미가 드러나지 않는다
-   반면 도메인 모델의 Orderer는 주문자라는 개념을 잘 반영하므로 도메인을 보다 잘 이해할 수 있도록 돕는다
-   1장에서 설명한 것처럼 밸류는 불변으로 구현할 것을 권장하며,
-   이는 엔티티의 밸류 타입 데이터를 변경할 때는 객체 자체를 완전히 교체한다는 것을 의미한다

#### 애그리거트

-   도메인 모델이 복잡해지면 전체 구조가 아닌 한 개 엔티티와 벨류에만 집중하는 상황이 발생하고
-   이때 상위 수준에서 모델을 관리하지 않고 개별 요소에만 초점을 맞추면, 큰 수준에서 모델을 이해하지 못해 큰 틀에서 모델을 관리할 수 없는 상황에 빠진다

-   도메인 모델을 상위 수준에서 볼 수 있어야 전체 모델의 관계와 개별 모델을 이해하는데 도움이 되고,
-   도메인 모델에서 전체 구조를 이해하는데 도움이 되는 것이 애그리거트 이다

-   애그리거트
    -   관련 객체를 하나로 묶은 군집
    -   주문 -> 주문, 배송지 정보, 주문자, 주문 목록, 총 결제 금액의 하위 모델로 구성된다
    -   이 하위 개념을 표현한 모델을 하나로 묶어서 '주문' 이라는 상위 개념으로 표현
-   애그리거트를 사용하면 개별 객체가 아닌 관련 객체를 묶어서 군집 단위로 모델을 바라볼 수 있게 되고
-   개별 객체가 아닌 애그리거트 간의 관계로 도메인 모델을 이해하고 구현할 수 있다
-   애그리거트는 군집에 속한 객체를 관리하는 루트 엔티티를 갖는다
    -   루트엔티티는 애그리거트에 속한 엔티티와 벨류 객체를 이용하여 애그리거트가 구현해야 할 기능을 제공
    -   애그리거트를 사용하는 코드는 루트를 통해서만 다른 엔티티나 벨류 객체에 접근이 가능하다
    -   이를 통해 내부 구현을 숨겨 애그리거트 단위로 구현을 캡슐화할 수 있도록 돕는다

#### 리포지터리

-   도메인 객체를 지속적으로 사용하려면 RDBMS, NoSQL 같은 물리적인 저장소가 필요하다
-   이를 위한 도메인 모델이 Repository
-   엔티티나 벨류가 요구사항에서 도출되는 도메인 모델이라면 리포지터리는 구현을 위한 모델
-   애그리거트 단위로 도메인 객체를 저장하고 조회하는 기능을 제공한다

#### 인프라스트럭처 개요

-   꼭 @Transactional 같은 인프라의 기술에 직접 의존하는 것이 나쁜 것은 아니다
-   구현의 편리함 또한 변경의 유연함이나 테스트가 쉬움 만큼 중요하기 때문에 DIP 의 장점을 해치지 않는 범위에서 응용 영역과 도메인 영역에서 구현 기술에 대한 의존을 가져가는 것은 나쁘지 않다고 한다
-   trade off를 잘 생각해서 하자

### 모듈 구성

-   아키텍처의 각 영역은 별도 패키지에 위치한다
-   도메인이 크면 하위 도메인으로 나누고 각 하위 도메인마다 별도 패키지를 구성한다
-   도메인 모듈은 도메인에 속한 애그리거트를 기준으로 다시 패키지를 구성한다
-   예를 들어 카탈로그 하위 도메인이 상품 애그리거트와 카테고리 애그리거트로 구성될 경우 도메인 패키지를 두 개의 하위 패키지로 구성할 수 있다
-   모듈 구조를 얼마나 세분화해야 하는지에 대해 정해진 규칙은 없다지만,
-   책의 저자는 한 패키지에 10~15개 미만으로 타입 개수를 유지하려 노력한다고 한다