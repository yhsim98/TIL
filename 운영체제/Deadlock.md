# Deadlock
* Blocked/Asleep state
    * 프로세스가 특정 이벤트를 기다리는 상태
    * 프로세스가 필요한 자원을 기다리는 상태
* Deadlock state
    * 프로세스가 발생 가능성이 없는 이벤트를 기다리는 경우
        * 프로세스가 deadlock 상태에 있음
    * 시스탬 내에 deadlock에 빠진 프로세스가 있는 경우
        * 시스템이 deadlock 상태에 있음
* deadlock과 starvation 차이
    * deadlock은 발생 가능성이 없는 상태 
    * starvation은 발생 가능하지만 무제한 대기상태

# 자원의 분류
* 일반적 분류
    * 하드워어 자원 vs 소프트웨어 자원

* 다른 분류법
    * 선점 가능 여부에 따른 분류
    * 할당 단위에 따른 분류
    * 동시 사용 가능 여부에 따른 분류
    * 재사용 가능 여부에 따른 분류

## 선점 가능 여부에 따른 분류
* 선점가능한 자원(Preemptible resource)
    * 선점 당한 후, 돌아와도 문제가 발생하지 않는 자원
    * Processor, memory 등
* Non-preemptible resource
    * 선점 당하면, 이후 진행에 문제가 발생하는 자원
        * Rollback, restart 등 특별한 동작이 필요
    * disk drive 등

## 할당 단위에 따른 분류
* Total allocation resources
    * 자원 전체를 프로세스에게 할당
    * Processor, disk drive 등
* Partitioned allocation resources
    * 하나의 자원을 여러 조작으로 나누어, 여러 프로세스들에게 할당
    * Memory 등

## 동시 사용 가능 여부에 따른 분류
* Exclusive allocation resources
    * 한 순가에 한 프로세스만 사용 가능한 자원
    * Processor, memory, disk drive 등
* Shared allocation resource
    * 여러 프로세스가 동시에 사용 가능한 자원
    * Program, shared data 등

## 재사용 가능 여부에 따른 분류
* SR(Serially-reusable Resources)
    * 시스템 내에 항상 존재하는 자원
    * 사용이 끝나면, 다른 프로세스가 사용 가능
    * Processor, memory, disk drive, program 등
* CR(Consumable Resources)
    * 한 프로세스가 사용한 후에 사라지는 자원
    * signal, message 등

# Deadlock과 자원의 종류
* Deadlock을 발생시킬 수 있는 자원의 형태
    * Non-preemptive resources
        * 만약 한번 할당받으면 끝날때까지 쓰기 때문에
        * preemptive 하다면 그냥 뺏으면 된다
    * Exclusive allocation resources
        * 누가 쓰고있으면 못쓴다
    * Serially reusable resources
        * CR도 발생시키지만 복잡하니 빼고 생각하자??
    * 할당 단위는 영향을 미치지 않는다

# Deadlock Model
## Graph Model
* Node
    * 프로세스 노드, 자원 노드
* Edge
    * R -> P : 자원이 프로세스에 할당됨
    * P -> R : 프로세스가 자원을 요청


## State Transition Model



# Deadlock 발생 필요 조건
* 자원의 특성
    * Exclusive use of resources
    * Non-preemptible resources
* 프로세스의 특성
    * Hold and wait(Partial allocation)
        * 자원을 하나 잡고 다른 자원 요청
    * Ciracular wait
        * 서로가 서로가 가지고 있는 것을 원함


# Deadlock 예방방법
4개의 발생조건을 제거, 비현실적


* 모든 자원을 공유 허용
    * 현실적으로 불가능
* Preemptible 허용
    * 현실적으로는 불가능
    * 비슷하게 구현해도 심각한 자원 낭비
* 필요 자원 한번에 모두 할당
    * Hold and wait 조건 제거
    * 자원 낭비 발생
        * 필요하지 않은 순간에도 가지고 있음
    * 무한 대기 현상 발생 가능
* Circular wait 조건 제거
    * Totally allocation을 일반화 한 방법
    * 자원들에게 순서를 부여
    * 프로세스는 순서의 증가 방향으로만 자원 요청 가능
        * 아래서부터 차레대로 요구
    * 자원 낭비 발생

# Deadlock Avoidance(회피)
deadlock 될 것 같으면 자원 할당 요청 보류, 시스템을 항상 safe state로 유지

* Safe state
    * 모든 프로세스가 정상적 종료 가능한 상태
    * Safe sequence 가 존재
        * Deadlock 상태가 되지 않을 수 있음을 보장
        * 무조건 발생안하는 건 아님
* Unsafe state
    * Deadlock 상태가 될 가능성이 있음
    * 반드시 발생한다는 의미는 아님

가정
* 프로세스의 수가 고정됨
* 자원의 종류와 수가 고정됨
* 프로세스가 요구하는 자원 및 최대 수량을 알고 있음
* 프로세스는 자원을 사용 후 반드시 반납한다
비현실적인 조건이기는 하다


## 다익스트라의 Banker's algorithm
* Deadlock avoidance를 위한 간단한 이론적 기법
* 가정
    * 한 종류의 자원이 여러 개라고 가정
* 시스템을 항상 safe state로 유지
* 현재 상태에서 안전 순서가 하나이상 존재하면 안전 상태, 하나도 못찾으면 unsafe 상태

* Banker's algorithm
    * 만약 자원을 줬을 때 safe 상태라면 자원을 준다
    * unsafe 하다면 주지 않는다
    * **시험문제 나온다면 safe가 존재함을 증명**하면 된다

## Habermann's algorithm
다익스트라의 확장, 자원의 개수가 여러개인 버젼

* high overhead
    * 항상 감시하고 있어야 함
* Low resource utilization
    * Safe state 유지를 위해, 사용 되지 않는 자원이 존재
* Non practical
    * 가정
        * 프로세스 수, 자원 수가 고정
        * 필요한 최대 자원 수를 알고 있음


# Deadlock detetction and deadlock recovery methods
## Deadlock Detection
* Deadlock 방지를 위한 사전 작업을 하지 않음
    * Deadlock 발생 가능
* 주기적으로 deadlock 발생 확인 
    * 어떤 시스템, 프로세서가 deadlcok 상태인지 알아야 한다
* Resource Allocation Graph(RAG) 사용
    * Deadlock 검출을 위해 사용
    * Directed, bipartite Graph
* 최선의 경우를 생각
?????


## Deadlock Recovery
* Process termination
    * Deadlock 상태인 프로세스 중 일부 종료
    * Termination cost model
        * 종료 시킬 deadlock 상태의 프로세스 선택
        * Termination cost에 따라 결정
            * 우선순위, 종료 등
    * Lowest-termination cost process first
        * simple
        * Low overhead
        * 불필요한 프로세스 종료 될 가능성 높다
    * Minimum cost recovery
        * 최소 비용으로 deadlock 상태를 해소 할 수 있는 process 선택
            * 모든 경우의 수를 고려해야 함
        * 복잡하다
        * High overhead 
            * O(2^k) 
* Resource preemption
    * Deadlock 상태 해결 위해 선점할 자원 선택
    * 해당 자원을 가지고 있는 프로세스에서 자원을 빼앗음
        * 자원을 빼앗긴 프로세스는 강제 종료 됨
        * Deadlock 상태가 아닌 프로세스가 종료될 수 있음
        * 해당 프로세스는 이후 재시작 됨
    * 선점할 자원 선택
        * Preemption cost model이 필요
        * Minimunum cost recovery method 사용
            * O(r)

* Checkpoint-restart method
    * deadlock를 해결하기 위해서는 process를 종료해야 함, 그때 재시작하기 위한 메소드
    * 프로세스의 수행 중 특정 지점마다 context를 저장
    * Rollback을 위해 사용
        * 프로세스 강제 종료 후, 가장 최근의 checkpoint에서 재시작






