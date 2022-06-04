# Redis
고성능 in-memory형 데이터베이스

key-value 방식으로 저장, String, list, hash, set등 다양한 자료구조를 지원합니다.

문자열 리스트 해시 등 다양한 방식의 타입을 지원

주로 cache 서버를 구현할 때 사용한다고 알고있습니다.

다양한 자료구조를 지원, 싱글스레드, 영속성 지원 등이 있습니다.

영속성
* RDB 방식
    * 순간적으로 메모리에 있는 내용을 디스크에 옮김
* AOF 방식
    * write update 연산을 log 파일에 기록

영속성을 지원안해서 Memcached 보다 느림, 싱글스레드, 더 다양한 자료구조, 멤캐시는 string이랑 integers만 지원

쓰기 연산은 느리지만 읽기 연산은 redis 가 더 빠르다
