# MSA(Micro Service Architecture)

마이크로서비스 아키텍처 스타일이란 단일 에플리케이션을 자체 프로세스로 실행되고 경량 메커니즘(주로 HTTP 리소스 API)으로 통신하는 작은 서비스들의 모음으로 개발하는 방식 - 마틴 파울러

비즈니스 시스템을 개발/운영/배포 할 때

- ONE THING
    - 한 가지 기능(비즈니스 관련 기능/역할)을 수행하는데 초점을 맞춘 서비스
- SMALL
    - 독립적이고 배포 가능한 가장 작은 단위의 서비스로 분리
- API
    - API를 통해 다른 서비스와 연계
- AUTONOMOUS
    - 각각 자율적으로 개발, 운영. 즉, 독립적인 팀이 각 서비스(atom)의 개발과 운영을 담당

## MSA란 무엇인가?

- 업무상의 기능 또는 역할을 하나의 기능 묶음으로 개발한 컴포넌트
- API Call을 통해 서비스 기능 연결
- 데이터를 공유하지 않고 서비스 별로 독립적으로 가공 및 저장

### MSA의 특징

- 분리된 서비스들의 집합
- 비즈니스 능력에 따른 기능 분할
- 똑똑한 엔드포인트와 파이프 처리
- 통제의 분권화
    - 각각의 MS별로 통제
- 데이터 관리의 분권화
    - 각 MS 별로 데이터 별도로 관리
- 자동화된 환경 구축
    - 서비스가 여러 개로 분리되다 보니
    - 자동화된 환경을 구축하지 않으면 구현이 어려워진다
- 장애에 대비한 설계
    - 특정 MS에 장애가 발생했을 때, 어떻게 대응할 것 인지에 대한 디자인이 들어있는 설계가 필요
- 변화에 대응할 수 있는 디자인
    - 일부분이 수정되더라도 빠르게 해당하는 부분만 수정해서 배포하면 된다

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7d279cd0-2c96-4172-a84c-52b01eebc00f/Untitled.png)

## MSA를 사용하는 이유

### 기존의 Monolitic 구조

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cd59858c-26be-4e1c-a2e4-d7c0cb477516/Untitled.png)

- 하나의 서비스의 오류가 다른 서비스에도 영향을 미치고,
- 배포하는데도 전체를 같이 해야 하기 때문에 시간이 오래 걸림

### MSA의 경우

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/245af40a-5c4e-492f-987e-9f54eff22938/Untitled.png)

- 하나의 서비스에 오류가 터져도 다른 서비스에 영향이 없다
- 해당하는 부분만 배포하면 된다
- 이용량이 많은 서비스에 대해서만 다중화를 하면 된다
    - scale out??

## MSA의 구성요소

### API Gateway

- 소프트웨어가 서로 의사소통을 하는 규약을 정해 놓은 것
- 특정한 task가 수행되는 방법을 표현
    - 어떠한 서비스가 호출되어야 하는 지
- 일반적 의미로는 운영체제, 애플리케이션, 라이브러리 등 다양한 수준의 인터페이스를 총칭 하는 것
    - MSA에서는 주로 특정한 서비스에 대해 웹 호출을 하는 그러한 Gateway을 제공하는 것
- REST-API와 같은 표준화 된 방식의 API을 사용하여 각 서비스/애플리케이션 간의 통신
- 표준화 된 방식을 사용하기 때문에 각 서비스의 구현은 언어의 제약을 받지 않고 구현 가능
    - ex) node, spring, golang 등등
- EAI/ESB의 대안 책으로 등장하였다
    - 서비스 간 통신을 간결한 JSON 양식을 주로 사용하기 때문에 ESB 대비 경량화

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/55a52500-ec2f-498d-a7f8-3943a4033235/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b6a7c1b5-c636-4f2c-b6f0-32358a79e97b/Untitled.png)

### API Gateway의 여러 구성요소

API Gateway의 여러 구성 요소를 살펴보자

### API Gateway Service

- 실제 라우팅과 인증, 로깅 등의 역할을 하는 부분
- 여러 필터로 구성되어 있다
- Spring Cloud Gateway, zuul, AWS API Gateway 등이 있다
- 밑은 Zuul Framework 구성요소이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/83bddb3d-6e89-4dc0-a5fa-a4c4a3f95d8d/Untitled.png)

- PRE Filter : 라우팅 전에 실행되는 필터로 주로 logging, 인증 등이 PRE Filter로 구성
    - 라우팅 : 알맞은 네트워크 경로를 선택
- ROUTE Filter : 요청에 대한 라우팅을 다루는 필터로  Apache httpclient를 사용하여 정해진 URL로 보낼 수 있고, Neflix Ribbon을 사용하여 동적으로 라우팅이 가능
    - 실제 micro service를 호출하는 역할
- POST Filter : 라우팅 후에 실행되는 필터로 response에 HTTP header를 추가하거나, response에 대한 응답속도, Status Code 등 응답에 대한 statistics and metrics을 수집
- ERROR Filter : 에러 발생 시 실행되는 필터

### Service Mesh

- MS간의 인프라 사이에서 원할하게 통신을 수행할 수 있도록 하는 인프라 계층
- 아키텍처 **내부**에서 요청이 어떤 지점으로 갈지 전달되는 방식을 추상화
- 서비스 메시를 통해 서비스 간의 통신을 추상화하여 안전하고 효율적이며 빠르게 구성할 수 있다
- service discovery, service registry, load balancing 등의 역할을 한다
- API Gateway과 client-to-server 사이라면, Service Mesh는 server-to-server 사이

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/438e05c3-2284-418f-b290-3964dca8a937/Untitled.png)

- 새로운 인스턴스가 실행되면 해당하는 인스턴스 정보를 API Gateway에서 알고 있어야 해당하는 MS로 요청을 보낼 수 있다
- Eureka는 이러한 인스턴스 정보를 수집해주는 서버

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/760bd12d-8e69-479d-9555-a164cfc734d8/Untitled.png)

- 밑은 로드 밸런싱 역할을 수행하는 Ribbon 이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/777fa7f0-b59d-4cf1-971c-c1e15e0397e2/Untitled.png)

- 일종의 Load Balancer
    - 일반적으로 라운드 로빈 방식 사용

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/abe06f93-1150-4ff7-b2aa-c579dfab3aa0/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/14485a6a-b0a2-4779-a87c-84b24f7d40e1/Untitled.png)

- 서킷 브레이커
    - 특정 API A가 API B를 호출할 때 항상 통신이 가능한 것은 아니다
    - B에 문제가 있을 경우 서비스 A에서 호출 후 응답을 기다리다, 타임 아웃이 일어나거나 에러 로그를 받아오거나 하는데
    - 계속해서 A가 B를 호출하게 되면  B가 마치 DDoS공격을 받는 것처럼 트래픽이 발생하게 되고 문제가 지속될 수 있다
    - 이런 것을 막아주는 것이 서킷브레이커

이렇게 API Gateway는 filter, 로드밸런서, 서킷브레이커 등 여러 요소들로 구성되어 있다.

## Micro Service

- 동일한 기능을 하는 기능을 묶어 놓은 최소 단위의 Service
- REST API 등을 통하여 서비스들의 기능 제공

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/08241789-81eb-4863-bca1-4f320d84f708/Untitled.png)

# MSA 도입 시 고려사항

### DB 트랜잭션 처리에 대한 고민

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/888658f7-adf8-4d3b-b913-a23783eb296c/Untitled.png)

- 여러 MS가 연결된 경우 트랜잭션 처리하기가 힘들다
- MSA의 겨우 서비스 별로 데이터, 즉 DB를 관리하게 된다

[MSA 분산 트랜잭션 관리](https://www.notion.so/MSA-5303f6892de746f38128e585b85015a9)

### Log 수집에 대한 고민

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e5bf00f1-cd05-4a34-afbe-872d9b0781a0/Untitled.png)

- 각각의 서버가 로그를 가지기 때문에 관리하기 힘들다
- 그래서 로그 수집기를 이용하여 로그를 모으고 분석기로 분석할 수 있다

### 오류 추적, 트랜잭션 추적에 대한 고민

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a18b0285-5ede-45f0-af8b-93a7b0f9217c/Untitled.png)

- 위의 Jaeger는 특정 마이크로 서비스를 지나갈 때마다 얼마나 시간이 들었는지 추적하는 툴
- 이런 식으로 어느 부분에서 문제가 일어났고 성능 병목이 있는지 등등을 추적하는 것을 고려해야 한다

# 추가

### Message Queue

- 대부분의 마이크로서비스에서는 메시지 큐를 활용한 비동기 패턴을 사용한다
-