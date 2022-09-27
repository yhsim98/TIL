# DynamoDB in node.js

### Query

- `Query`는 기본 키 값을 기반으로 항목을 찾습니다
- 파티션 키 속성의 이름과 해당 속성을 단일 값을 제공해야 하고, `Query` 는 해당 파티션 키 값을 갖는 모든 항목을 반환합니다
- 선택적으로 정렬 키 속성을 제공하고 비교 연산자를 사용하여 검색 결과를 구체화할 수 있습니다
- `KeyConditionExpression` 을 이용하여 파티션 키의 값을 구체화할 수 있습니다

### 주제

- [쿼리에 대한 키 조건 표현식](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/Query.html#Query.KeyConditionExpressions)
- [쿼리에 대한 필터 표현식](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/Query.html#Query.FilterExpression)
- [결과 세트의 항목 수 제한](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/Query.html#Query.Limit)
- [테이블 쿼리 결과 페이지 매김](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/Query.Pagination.html)
- [결과 내 항목 수 계산](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/Query.html#Query.Count)
- [쿼리에서 사용된 용량 단위](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/Query.html#Query.CapacityUnits)
- [쿼리에 대한 읽기 정합성](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/Query.html#Query.ReadConsistency)

## DynamoDB paging

- `LastEvaluatedKey` 를 사용합니다
- 이전 결과의 `LastEvaluatedKey` 를 `ExclusiveStartKey` 에 넣으면 다음 결과가 나옵니다
- 만약 결과값의 `LastEvaluatedKey` 가 결과값에 없다면 마지막 페이지 입니다
- 아래는 조회 값입니다

```tsx
Count: 3,
ScannedCount: 3,
LastEvaluatedKey: {
userId: '1',
requestedAt: '2022-09-27 11:30:16',
requestId: '2bccf83a-9af8-4f29-ba7c-d28e20ff0d30'
}
```

- 

## Global secondary Index

- 기본 키 속성 외에 다른 속성으로 조회할 경우 `Scan` 을 해야 합니다

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3baf9c12-9127-4dc0-98ee-a59e4526b14a/Untitled.png)

- 예를 들어 각 유저의 최고 점수나, 최고 승률 등을 계산하는 등등
- 키가 아닌 속성에 대한 쿼리 속도를 높이기 위해 글로벌 보조 인덱스를 만들 수 있습니다
- 글로벌 보조 인덱스는 기본 테이블의 속성 중 일부를 포함하지만 테이블과 다른 기본 키를 기준으로 구성됩니다
    - 인덱스로 사용할 새로운 테이블을 생성
    - 원하는 속성을 파티션 키로 하는 테이블

  

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cabc246b-9e66-4483-9667-a8dd50e568b7/Untitled.png)

- 해당 글로벌 보조 인덱스를 통해

## 파티션 키

- Primary Key, 단일 primary key 이다
- 복합 primary key를 지정하고 싶으면 sork key를 사용하면 된다
- key type 은 `HASH` 입니다
- Hash 타입이기 때문에 일치 방식의 검색만 지원하게 됩니다
- 또한 Hash type key를 이용하여 sharding을 수행합니다

## 정렬 키

- 파티션 키는 물리적 공간인 파티션을 특정하는 키
    - 스케일이 아무리 커져도 주소를 알고 있어서 빠르게 가져올 수 있다
- 정렬 키는 파티션 내에서 정렬하는 기준 값, 같은 파티션키라면 정렬값을 기준으로 정렬되서 저장됩니다
    - Number, Binary, String 타입을 지원하며, String 이라면 utf-8 기준으로 정렬됩니다
    - 단순정렬이기 때문에 파티션의 사이즈가 커도 빠르게 가져올 수 있습니다
- sort key는 range type key 입니다

```tsx
KeySchema=[
{
"AttributeName": "Author",
"KeyType": "HASH"
},
{
"AttributeName": "Title",
"KeyType": "RANGE"
}
]
```

- 기본 키가 해시방식이기 때문에 범위 연산을 지원하지 않고, 이것을 보완하기 위해 사용하는듯?