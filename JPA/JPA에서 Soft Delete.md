# JPA에서의 Soft Delete
## ``@Where``
테이블에 ``@Where`` 어노테이션을 추가하면 일괄적인 Where 조건이 적용된다.

```
@Entity
@Where(clause = "column_name=condition")
public Entity{
}
```

엔티티에 해당 어노테이션을 붙이면 

해당 엔티티를 조회하는 모든 쿼리에 설정한 where 문이 붙게 된다.

Lazy Loading을 하는 경우에도, 조인을 하는 경우에도 해당 Where 문이 적용되게 된다.

## ``@SQLDelete``
엔티티에 해당 어노테이션을 추가하면 삭제가 이뤄질 때 수행할 쿼리를 지정할 수 있다.

```
@SQLDelete("update table_name SET is_deleted=1 WHERE id = ?")
@Entity
public Entity{
}
```

```@SQLDelete("")``에 삭제시 수행될 쿼리를 작성하면 된다.


## Soft Delete 구현하기
``@Where``어노테이션과 ``@SQLDelete`` 어노테이션을 결합하면 soft delete 를 쉽게 구현할 수 있다.

soft delete 쿼리를 ``@SQLDelete``를 통해 수행하고, 삭제된 데이터를 가져오지 않는 조건을 ``@Where``을 통해 적용하면 간단히 구현 가능할 뿐만 아니라 실수의 여지도 줄어든다.

