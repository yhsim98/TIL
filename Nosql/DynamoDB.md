# DynamoDB

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

# Local Secondary Index

- 간단하게 설명하면, 같은 파티션 키 값에 다른 정렬 키

![]([https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/images/LSI_01.png](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/images/LSI_01.png))

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ae83efa7-17c0-4642-ac78-7cb4f2dd904b/Untitled.png)

- DynamoDB는 파티션 키 값이 동일한 모든 항목을 연속적으로 저장합니다
- 위 예에서는 특정 `ForumName` 에 의해 `query` 작업으로 해당 포럼의 모든 스레드를 즉시 찾을 수 있습니다
- 그리고 동일한 파티션 키 값을 가진 항목 그룹 내에서는 항목이 정렬 키 값을 기준으로 정렬됩니다

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

## 핫 파티션

- 테이블 기본 키의 파티셔 키 부분은 테이블의 데이터가 저장되는 논리적 파티션을 결정합니다
- 이는 기본 물리적 파티션에도 영향을 줍니다
- I/O 요청을 고르게 분산시키지 않는 파티션 키 설계는 ‘핫’파티션을 발생시킬 수 있으며, 이는 프로비저닝된 I/O용량을 비효율적으로 사용하게 되는 문제를 초래합니다

- 테이블의 프로비저닝된 처리량의 최적 사용량은 개별 항목의 워크로드 패턴과 파티션 키 설계가 결정합니다
    - 각 파티션 별로 I/O 작업량을 할당
- 하나의 파티션에 작업이 자주 발생할 경우 전반적인 성능 저하를 야기하는 핫 파티션 문제가 발생할 수 있습니다

# DynamoDB 쿼리 작업

- dynamoDB에서 query작업은 기본 키 값을 기반으로 항목을 찾습니다
- 파티션 키 속성의 이름과 해당 속성의 단일 값을 제공해야 합니다
- query는 해당 파티션 키 값을 갖는 모든 항목을 반환합니다
- 정렬 키 속성과 비교 연산자를 사용하여 검색 결과의 범위를 좁힐 수 있습니다

## 쿼리에 대한 키 조건 표현식

- `a = b, a < b, a <= b, a > b, a >= b`
- `a BETWEEN b AND c` : a가 b보다 크거나 같고 c보다 작거나 같은 경우
- `begins_with (a, substr)` : a 속성 값이 특정 하위 문자열로 시작하는 경우 true

## node aws-sdk DynamoDB client 에서 예시

```jsx
const query = {
    TableName: TABLE_NAME,
    IndexName: USER_KEY_INDEX_NAME,
    Limit: 1,
    ScanIndexForward: false,
    KeyConditionExpression: "userId = :userId",
    ExpressionAttributeValues: {
        ":userId": userId
    },
}
```

- 특정 파티션 키(userId)값을 갖는 항목을 모두 읽어옵니다

```jsx
const query = {
    TableName: TABLE_NAME,
    IndexName: USER_KEY_INDEX_NAME,
    Limit: 1,
    ScanIndexForward: false,
    KeyConditionExpression: "userId = :userId and expiredAt < now and remainPoint > 0",
    ExpressionAttributeValues: {
        ":userId": userId
    },
}
```

- 파티션 키(userId)값을 가지면서도 expiredAt(정렬키) < now && remainPoint(정렬키) > 0, 인 값을 가져옵니다

- 일정한 파티션 키 값을 갖는 항목들에 대해서는 DynamoDB가 정렬 키 값을 기준으로 순서를 정렬하여 모두 함께 저장합니다
    - `Query` 작업을 할 때는 정렬된 순서대로 항목을 가져온 다음 `FilterExpression` 과 모든 `KeyConditionExpression` 을 사용해 항목을 처리합니다
    - 클러이언트에게는 `Query` 결과만 다시 보내집니다
    - 단일 `Query` 작업은 최대 1MB의 데이터를 가져올 수 있습니다
- 기본적으로 조회 결과는 sort key를 기준으로 오름차순 정렬되어 조회됩니다
    - 내림차순 정렬을 원한다면 `ScanIndexForward` 를 false로 하면 됩니다

## 쿼리에 대한 필터 표현식

- `Query` 결과를 한층 더 좁혀야 하는 경우 선택적으로 필터 표현식을 제공할 수 있습니다
- 필터 표현식은 `Query` 완료 후 적용되기 때문에, 필터 표현식의 여부와 상관없이 `Query` 는 동일한 양의 읽기 용량을 사용합니다