## 데이터베이스를 설계할 때 ERD 도구로 어떤 것을 사용했나요?
저는 viso 를 사용했습니다. 아는게 이거밖에 없기도 하고 딱히 사용하는데 불만은 없습니다.

## DB 락의 종류
읽기 전용인 경우 사용하는 Shared Lock과 데이터를 수정할 때 사용하는  Exclusive Lock이 있습니다.

shared Lock의 경우는 같은 shared Lock 끼리는 접근을 혀용하지만 Exclusive Lock의 접근은 막습니다.

Exclusive Lock의 경우 모든 접근을 막습니다. 데드록이나 블로킹이 발생가능합니다.



