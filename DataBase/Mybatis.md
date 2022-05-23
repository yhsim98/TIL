# Collection
1:N 관계에서 서브쿼리 형식으로 데이터를 조회해 준다.

Board와 Comment가 있고 Board객체가 Comment List인 Comments를 가지고 있다면,

Collection을 사용하지 않을 때 Board를 조회한 다음, Board의 id를 이용하여 Comments를 따로 조회해야 한다. 이것은 DB에 두번이나 접근해야 하므로 좋지 않다.

Collection을 사용하게 되면 서브 쿼리 형식으로 조회하여 매핑해 주기 때문에 한 번만 접근해도 된다.

# Associtaion
1:N 관계에서 N 측의 경우 1측의 관계를 나타내는 속성을 가지게 된다.

보통 id인데 만약 객체지향스럽게 1 측의 객체를 직접 가지고 있는 경우 단순히 select로 조회해서는 매핑하기 힘들다.

이 경우 resultMap의 association을 활용하면 간단하게 조회하여 매핑할 수 있다.

