//https://mangkyu.tistory.com/118

# Garbage Collector
자바에서 객체는 힙 영역에 저장되고 포인터는 스택 영역에 저장되어 변수가 그것을 가리킨다. 

하지만 변수가 가리키는 객체가 변경되어 더 이상 힙 영역의 객체를 아무도 참조하지 않는다면 GC가 해당 객체를 자동으로 반납하게 된다.

## Minor GC와 Major GC
JVM의 Heap 영역은 처음 설계될 때 다음의 2가지를 전제로 설계되었습니다.

* 대부분의 객체는 금방 접근 불가 상태가 된다
* 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다

**객체는 대부분 일회성이며, 메모리에 오랫동안 남아있는 경우는 드뭅니다.** 그렇기 때문에 객체의 생존 시간에 따라 물리적 Heap영역을 Young과 Old로 나누게 되었습니다.

* Young 영역
    * 새롭게 생성된 객체가 할당되는 영역
    * 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에 생성되었다 사라진다
    * Young 영역에 대한 가비지 컬렉션을 Minor GC라고 부른다
* Old 영역
    * Young 영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역
    * 복사되는 과정에서 대부분 Young 영역보다 크게 할당되며, 크기가 큰 만큼 가비지는 적게 발생한다
    * Old 영역에 대한 가비지 컬렉션을 Major GC또는 Full GC이라 부른다

보통 Old 영역은 Young 영역보다 크게 할당됩니다. Young 영역의 수명이 짧은 객체들은 큰 공간을 필요로 하지 않고 큰 객체들은 Young영역이 아니라 바로 Old 영역에 할당되기 때문입니다.

* 예외적인 상황으로 Old 영역의 객체가 Young영역의 객체를 참조하는 경우가 있다
* 그래서 Old 영역에는 Card Table이 존재한다
* Card Table에는 Old영역의 객체가 Young영역의 객체를 참조할 때 그에 관한 정보가 저장된다
* Minor GC시 Card Table만 검사해도 GC대상인지 식별할 수 있다

## 종류
* Serial GC
    * GC를 처리하는 쓰레드가 1개
    * 다른 GC에 비해 stop-the-world가 길다
    * Mark-Compact 알고리즘 사용
        * mark-sweep-compact?
        * old영역에서 살아있는 객체를 모은다
* Parallel GC
    * Java8의 default GC
    * Young Generation의 GC을 멀티 쓰레드로 수행
    * serial에 비해 stop-the-world 시간 감소
* Parallel old GC
    * Parallel GC 개선
    * Old 에도 멀티스레드
    * Mark-summary-compact 알고리즘 사용
    * 이전 GC에서 살아있는 객체의 위차를 조사
* CMS(Concurrent Mark Sweep) GC
    * stop-the-world 시간을 줄이기 위해 고안됨
    * compact 과정이 없음
*  G1 GC
    * Heap을 일정한 크기의 Region으로 나눔
    * 전체 Heap이 아닌 Region단위로 탐색
    * compact 진행