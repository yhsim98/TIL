https://khj93.tistory.com/entry/Spring-Batch%EB%9E%80-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0

# Spring Batch
* 로깅/추적, 트랜잭션 관리, 작업 처리 통계, 작업 재시작, 건너뛰기, 리소스 관리 등 대용량 레코드 처리에 필수적인 기능을 제공
* 최적화 및 파티셔닝 기술을 통해 대용량 및 고성능 배치 작업을 가능하게 하는 고급 기술 서비스 및 기능을 제공
* Spring Batch 에서 배치가 실패하여 작업 재시작을 하게 된다면 처음부터가 아닌 실패한 지점부터 실행

## Spring Batch vs Quartz? Scheduler? 
Spring Batch는 Scheduler가 아니기에 비교 대상이 아닙니다.

Spring Batch는 Batch Job을 관리하지만 Job을 구동하거나 실행시키는 기능은 지원하지 않습니다.

Batch Job을 실행기키기 위해서는 Scheduler, Jenkins등 전용 Scheduler을 사용하여야 합니다.


# Spring Batch 용아
### Job
* 배치처리 과정을 하나의 단위로 만든 객체
* 배치처리 과정의 최상단


### JobInstance
* Job의 실행 단위를 나타냅니다
* Job을 실행시키게 되면 하나의 JobInstance가 생성
    * JobInstance가 실패하면 해당 JobInstance만 재실행

### JobParameters
* JobInstance를 구별할 수 있게 해준다
* JobInstance에 전달 되는 매개변수 역할??
* String, Double, Long, Date 4가지 형식을 지원합니다

### JobExecution
* JobInstance에 대한 실행 시도에 대한 객체입니다
* JobInstance가 실패하면 동일한 JobInstance를 실행시키지만 각 시도에 대한 JobExecution은 개별로 생깁니다
* 실행 상태, 시작시간, 종료시간, 생성시간 등의 정보를 담고 있습니다

### Step
* Job의 배치처리를 정의하고 순차적인 단계를 캡슐화합니다
* Job은 최소한 1개 이상의 Step을 가져야 합니다
* 실제 일괄 처리를 제어하는 모든 정보가 들어있습니다

### StepExecution
* Step의 실행 시도에 대한 객체
* Job이 여러 Step으로 구성되어 있을 경우 이전 단계의 Step이 실패하게 되면 다음 단계가 실행되지 않음으로 실패 이후 StepExecution은 생성되지 않습니다
* 실제 시작이 될 때만 생성됩니다
* StepExecution은 JobExecution에 저장되는 정보 외에 read 수, write 수, commit 수, skip 수 등의 정보들도 저장이 됩니다

그 외 등등

# Spring Batch 사용하기
* Spring Batch에서의 Job은 여러가지 Step의 모음으로 구성되어 있습니다
* Job은 순차적인 Step을 수행하며 Batch를 수행하게 됩니다
* Step은 Tasklet 처리 방식과 Chunk 지향 처리 방식을 지원하고 있습니다.

