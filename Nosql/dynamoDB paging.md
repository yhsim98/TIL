## DynamoDB Query 조회

-   `Query`는 기본 키 값을 기반으로 항목을 찾습니다
-   파티션 키 속성의 이름과 해당 속성을 단일 값을 제공해야 하고, `Query` 는 해당 파티션 키 값을 갖는 모든 항목을 반환합니다
-   선택적으로 정렬 키 속성을 제공하고 비교 연산자를 사용하여 검색 결과를 구체화할 수 있습니다
-   `KeyConditionExpression` 을 이용하여 파티션 키의 값을 구체화할 수 있습니다
-   dynamoDB Client의 query 조회의 반환값은 아래의 4가지 속성을 반환합니다
-   만약 더 이상 조회할 item이 없다면 LastEvaluatedKey는 반환하지 않습니다.

```
DocumentClient.queryOutput?? {
    Items 
    Count
    ScannedCount
    LastEvaluatedKey?
}
```

## paging

-   DynamoDB는 cursor 방식의 페이징을 제공합니다
-   위의 LastEvaluatedKey를 이용하여 페이징할 수 있습니다.
-   이전 결과의 LastEvaluatedKey 를 ExclusiveStartKey 에 넣으면 다음 결과가 나옵니다
-   만약 결과값의 LastEvaluatedKey 가 결과값에 없다면 마지막 페이지 입니다
-   아래는 조회 값입니다

```
Items: [.....],
Count: 3,
ScannedCount: 3,
LastEvaluatedKey: {
    userId: '1',
    requestedAt: '2022-09-27 11:30:16',
    requestId: '2bccf83a-9af8-4f29-ba7c-d28e20ff0d30'
}
```

-   위의 LastEvaluatedKey를 다음 조회의 ExlusiveStartKey에 넣습니다
-   아래는 제가 직접 페이징을 구현한 코드의 일부입니다
-   query를 보시면 ExclusiveStartKey에 cursor를 넣는 것을 볼 수 있습니다
    -   cursor가 LastEvaluatedKey 입니다
    -   만약 null이 들어간다면 첫번째값부터 조회합니다

```
getPointHistoriesByUserId = async (userId: string, cursor: PointCursor, pageSize: string): Promise<any> => {
        const query = {
            TableName: TABLE_NAME,
            IndexName: INDEX_NAME,
            ExclusiveStartKey: cursor,
            Limit: Number(pageSize),
            KeyConditionExpression: "userId = :userId",
            ExpressionAttributeValues: {
                ":userId": userId
            }
        };

        const queryResult = await getDynamoDBs(query);

        const result: DynamoGetItemsResult<PointLogDTO> = {
            Items: queryResult.Items,
            Count: queryResult.Count,
            ScannedCount: queryResult.ScannedCount,
            LastEvaluatedKey: queryResult.LastEvaluatedKey
        };

        result.contents = result.Items.map(item => new PointLogDTO(item));

        return new Paging<PointLogDTO>(result, Number(pageSize), true);
}


interface PointCursor {
    userId: string;
    requestedAt: string;
    requestId: string;
}
```

## cursor를 클라이언트에 반환

-   HTTP 통신은 stateless 함으로 다음 조회를 위해 cursor를 클라이언트에 반환하고, 조회시 다시 cursor를 받을 필요가 있습니다
-   이때 복합속성인 `LastEvaluatedKey`를 그대로 반환하면 클라이언트 서버 양쪽다 굉장히 번거로워집니다
    -   조회 시 cursor를 url 쿼리로 받아야 하는데 힘들어집니다..
-   그래서 해싱을 통해 encode 하여 클라이언트에 전달해 줍니다

```
export const base64Encoding = (obj: object): string => {
    return Buffer.from(JSON.stringify(obj), 'utf-8').toString('base64');
}

export const base64Decoding = (str: string) => {
    return JSON.parse(Buffer.from(str, 'base64').toString('utf-8'));
}

class Paging<T> {
    content: T[];
    pageSize: number;
    count: number;
    sorted: boolean;
    cursor: string;

    constructor(object: DynamoGetItemsResult<T>, pageSize: number, sorted: boolean) {
        this.content = object.contents;
        this.pageSize = pageSize;
        this.count = object.Count;
        this.sorted = sorted;
        this.cursor = object.LastEvaluatedKey === undefined ? null : base64Encoding(object.LastEvaluatedKey);
    }
}
```

-   위의 paging 클래스의 생성자를 보시면 `LastEvaluatedKey`를 받아 base64로 인코딩하고 있습니다.
-   이렇게 반환하면 반환하기도 쉽고, 클라이언트로부터 요청받을 때도 query로 간단하게 받을 수 있습니다
    -   `http://localhost:8000/points?cursor=asdasdasdas=&pageSize=10`