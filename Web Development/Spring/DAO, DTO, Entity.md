# DAO 패턴
Data Access Object의 약자, repository package
* 실제로 DB에 접근하는 객체다
    * Persistence Layer(DB에 data를 CRUD하는 계층)이다
* service와 DB를 연결하는 고리의 역할을 한다
* 데이터 엑세스 계층은 DAO 패턴을 적용하여 **비즈니스 로직과 데이터 액세스 로직을 분리**하는 것이 원칙이다
* DAO 패턴은 서비스 계층에 영향을 주지 않고 데이터 엑세스 기술을 변경할 수 있다는 장점이 있다

# DTO란?
Data Transfer Object의 약자
* 계층간 데이터 교환을 위한 객체(Java Beans)이다
    * DB에서 데이터를 얻어 Service나 Controller 등으로 보낼 때 사용하는 객체이다
    * 즉, DB의 데이터가 Presentation Logic Tier로 넘어오게 될 때는 DTO의 모습으로 바껴서 오고간다
    * *로직을 갖고 있지 않는 순수한 데이터 객체*이며, getter/setter 메서드만을 갖는다
    * 하지만 DB에서 꺼낸 값을 임의로 변경할 필요가 없기 때문에 DTO클래스에는 setter가 없다(대신 생성자에서 값을 할당한다)
* Request와 Response용 DTO는 View를 위한 클래스 
    * 자주 변경이 필요한 클래스
    * Presentation Model
    * toEntity() 메서드를 통해서 DTO에서 필요한 부분을 이용하여 Entity로 만든다     
    * 또한 Controller Layer에서 Response DTO 형태로 Client에 전달한다
* VO(Value Object)
    * VO와 DTO는 동일한 개념이지만 read only 속성을 갖는다
    * VO는 특정한 비즈니스 값을 담는 객체이고, DTO는 Layer간의 통신 용도로 오고가는 객체를 말한다


# Entity 클래스
* 실제 DB의 테이블과 매칭될 클래스
    * 테이블과 링크될 클래스
    * Entity 클래스 또는 가장 Core한 클래스라 한다
    * @Entity, @Column, @Id들을 이용한다
    * 쿼리를 날리기 보다는 Entity 클래스의 수정을 통해 DB 데이터를 작업한다
* 최대한 외부에서 Entity 클래스의 getter method를 사용하지 않도록 해당 클래스 안에서 필요한 로직을 구현해야 한다
    * 단 Domain Logic만 가지고 있어야 하고 Presentation Logic을 가지고 있어서는 안된다
    * 여기서 구현한 메소드는 주로 Service Layer에서 사용한다
* 절대 Setter 메소드를 만들지 않는다 
    * 필드의 값 변경이 필요하면 명확히 그 목적과 의도를 나타낼 수 있는 메소드를 추가해야만 한다
* Entity 클래스와 DTO는 유사한 형태이지만 분리되어야 한다
    * view(web) Layer과 DB Layer의 역할을 철저하게 분리하기 위해서이다
    * Entitiy 클래스를 Request / Response 클래스로 사용해서는 안된다
    * Entity 클래스는 **데이터베이스와 맞닿은 핵심 클래스**로써 Entity 클래스를 기준으로 테이블이 생성되고, 스키마가 변경된다
    * Entity 클래스를 기준으로 많은 서비스나 비즈니스 로직들이 동작한다 하지만 Request와 Response용 Dto는 View를 위한 클래스라 자주 변경이 필요한데, 이를 위해 테이블과 연결된 Entity를 수정하는 것은 너무 큰 손해이다
