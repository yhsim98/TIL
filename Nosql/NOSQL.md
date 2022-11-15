# NoSQL

- 비관계형 데이터베이스 : NoSQL, Not Only SQL
    - 정형화된 데이터를 저장하지 않는 데이터 저장소
    - 문서 등 다양한 형식의 데이터를 저장가능
        - key, value
        - 문서
        - 등등
- Schema-Free : 보다 유연한 데이터 모델
- 분산다수의 하드웨어에 걸쳐 저장된 모델

- RDBMS는 데이터를 정형화시켜야 한다
- ACID 트랜잭션 위주
    - NoSQL은 BASE
    - Basically Available : 다수의 스토리지에 복사본 저장, 실패에도 가용성
    - Soft-State : 분석 노드 간 업데이트는 데이터가 노드에 도달한 시점에 갱신
    - Eventually Consistency : 일시적으로 비일관적이라도 최종으로는 일관성
- 은행 등등 실시간 운영계 시스템에서는 ACID 트랜잭션을 사용해야 함

- CAP 이론
    - Consistency
    - Availability
    - Partition Tolerance
    - 이 중에 2가지만 가질 수 있다



cf. SQL DB: MySQL, PostgreSQL, SQLite

NoSQL은 클라우드 환경에 맞는 저장 기술

직접 프로그래밍 하는 등의, 비 SQL 인터페이스를 통한 데이터 액세스

기존 RDBMS의 특성에 더해 부가적 기능 지원

테이블 간 관계를 정의하지 않으며, 일반적으로 테이블 간 Join 불가능

데이터 일관성은 포기하되 비용을 고려해 여러 대의 DB에 분산하여 저장하는 Scale-Out을 목표로 등장

트랜잭션 ACID(원자성, 일관성, 고립성, 영구성)를 제공하지 않음 - 데이터 무결성, 정합성 보장하지 않음

비정형, 반정형 데이터 처리 - SNS, 로그성 데이터에 적합 (→ SNS 데이터는 RDB에도 저장 가능한 거 아닌가)

- 정형, 비정형, 반정형
    
    정형 데이터(Structured Data): 수치만으로 의미 파악이 쉬운 데이터 ex. Gender, Age
    
    비정형 데이터(Unstructured Data): 정해진 규칙이 없어 의미를 파악하기 어려운 데이터 ex. 텍스트(제목/본문), 음성, 영상
    
    반정형 데이터(Semi-structured Data): 약한 정형 데이터 ex. HTML, XML
    
    - 일반적으로 데이터 베이스는 데이터를 저장하는 장소(테이블)와 스키마(데이터)가 분리되어 있어서 테이블을 생성하고 데이터를 저장하는데, 반정형은 하나의 텍스트 파일에 Column과 Value를 모두 출력 ex.”Sepal.Length” : 6.8, “Petal.Length” : 5.9
    
    - 데이터를 주고 받을 때 변경된 포맷 이상의 의미를 갖지 않음
    

약한 Consistency(일관성)

데이터의 스키마와 속성들을 다양하게 수용 및 동적 정의(Schema-less)

데이터베이스의 중단 없는 서비스와 자동 복구

일관성보다는 동시성,ㅌ 대량 데이터 처리(확장성, 가용성), 빠른 성능, 분산 DB(여러 개의 DB 서버를 묶어 하나의 DB 구성 = 클러스터링)

기존 RDB보다 더 융통성 있는 데이터 모델, 데이터의 저장 및 검색에 특화된 매커니즘 → 단순 검색 및 추가 작업에 최적화된 키 값 저장 기법 사용, 응답 속도와 처리 효율에 뛰어난 성능.

⇒ 초고용량 데이터 처리 등 성능에 특화된 목적을 위해 비관계형 데이터 저장소에 비구조적 데이터 저장하는 분산 저장 시스템

- NoSQL의 종류
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/14e17fe2-6224-4da0-b24a-c87a7cc056ac/Untitled.png)
    
    - Key Value DB: **Dynamo**, Riak, Vodemort, Tokyo
        - Dynamo DB는 서버리스, 분산된 Key-Value DB. 아마존, 듀오링고.
    - Wide Columnar Store(Big Table DB): Key Value에서 발전된 Column Family 데이터 모델 - **HBase**, **Cassandra**, **ScyllaDB**
        - Cassandra는 Key Value DB이기도 함. 읽고 쓰기가 빠르다. 애플, 넷플릭스, 인스타그램, 우버, 검색엔진.
        - 저장하기 전에 DB에서 무엇을 얻을지 정해둬야 함. SQL은 데이터 구조만 짜둘 뿐, 나중에 어떻게 뽑아 쓸지는 고민할 필요 없음.
    - Document DB: JSON, XML과 같은 Collection 데이터 모델 - **MongoDB**, CoughDB
        - 보통의 SQL과 같은 행과 열이 아닌, 데이터를 Json document 형태로 저장
        - Column이나 Document가 필요 없고 노드 간의 관계만 필요할 때(SNS). 페이스북 Tao. 각각의 entity를 저장하고 관계망으로 연결.
    - Graph DB: Nodes, Relationship, Key-Value 데이터 모델 - Neo4J, OrientDB
    - Data-Structures: **Redis**