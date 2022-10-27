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