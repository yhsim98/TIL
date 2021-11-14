# DNS(domain name service)

# 도메인명이란?
도메인명(Domain name)
* 인터넷 호스트에 부여되는 문자형의 유일한 이름
* 계층적인 도메인 관리 구조에 의해 도메인명의 유일성 유지
* 도메인 관리자가 상위 도메인 관리에게 등록한 후 사용

도메인 구조
* 최상위 도메인 : 7개 일반 도메인
    * .com, .org, .net, .int, .edu, .gov, .mil
    * 국가 도메안 : .kr, .jp, .uk ....
* 중간 도메인 
    * .ac, .gov, .re, .or...
* 책임 도메인
    * .koreatech.ac.kr
* 호스트 도메인 
    * sce.koreatech.ac.kr, scpark.koreatech.ac.kr

계층적인 도메인 구조
* Root 도메인 -> TLD(top level domain)(최상위 도메인 둘 중 하나)(최상위 관리 기관)  ->  중간 도메인(중간 관리 기관)
* 상위 기관에서 하위 기관으로 유일한 이름을 가지도록 위임하게 된다.

도메인명 특징
* 사용자 편리성
* 사용자 소속성

# DNS 서비스 유형
* Hostname to IP Address(호스트명 - IP 주소 변환 서비스)
    * 사용자의 문자형의 호스트명(도메인명)을 TCP/IP가 사용하는 32비트 IP 주소로 변환
* Host aliasing(호스트 별칭 서비스)
    * 사용자의 호스트 별칭(alias)을 복잡한 정규 호스트명으로 변환
    * Host aliasing -> hostname -> IP 
* Mail server aliasing(메일서버 별칭 서비스)
    * 사용자의 메일서버 별칭을 복잡한 정규 호스트명으로 변환
* Load distribution(부하 분산 서비스)
    * 동일 서버명(도메인명)으로 서로 다른 IP 주소의 다중 복제 서버 배치
    * 동일 서버명에 대한 DNS 요청에 대해 다른 IP 주소를 돌아가며 응답

# DNS 구조
## 중앙집중형 구조
하나의 서버가 모든 DNS 질의 처리

문제점
* A single point of failure
    * 중앙 서버 고장 시 전체 네트워크 고장
* Traffic volume 
    * 중앙 서버에 부하 집중
* Distant centralized database
    * 원거리 중앙 서버의 응답 지연시간 증가
* Maintenance 
    * 중앙 서버 유지 보수 어려움

## 분산 계층 구조
계층 구조로 관리하게 된다.  
도메인 구조의 책임 서버가 각자의 DNS 처리한다. 

루트 서버가 TLD 서버 IP주소 TLD 서버가 그 아래 주소 ....

## 루트 서버
* 일반적으로 중간 서버(TLD)에 대한 IP 주소 제공
* 전서계에 수백개의 복제 서버 존재
* 13개의 관리 기관에 의해 관리
* 지역(local) DNS 서버에 의해 처음 접속

## 책임 DNS 서버
* 특정 호스트의 도메인명 정보를 유지하고 있는 서버
* 규모가 큰 기관은 대부분 자신의 호스트들에 대한 책임 DNS 서버 유지
* 소규모 기관은 외부의 책임 DNS 서버 위탁

## 지역(local) DNS 서버
* DNS 서비스를 요청하는 클라이언트 호스트가 소속된 DNS 서버
* 소속된 호스트들의 DNS 서비스 요청 대행
* 처음의 클라이언트 요청을 받아 root DNS 서버로 연결한다

## 반복적 질의
1. 호스트가 local DNS server로 요청을 하면 local 서버는 root 서버로 요청을 보내고 응답을 받는다.
2. local 서버가 응답을 받으면 TLD 서버로 다시 요청을 보내고 응답을 받는다.
3. 응답을 받은 local 서버가 책임 DNS서버로 요청 보내고 응답 받는다.
4. local서버가 호스트에 최종적인 domain name을 응답해 준다.

## 재귀적 질의
반복적 질의와 달리 local에서 시작하여 상위 책임부터 하위 책임까지 재귀적 방식으로 요청을 보내고 응답을 받아 최종적으로 local에 domain name이 모여 호스트에게 반환된다.

# DNS 서비스 제공 방식
DNS 캐싱
* 특정 서버가 다른 서버에게 질의를 하고 응답을 받으면 이 정보를 클라이언트 방향으로 전달하기 전에 자신의 캐쉬 메모리에 저장
* 해당 서버가 동일한 도메인명에 대한 해석을 요청하는 질의를 수신하면 다른 서버에게 질의 메시지를 전달하는 대신 캐쉬 메모리 정보를 응답

문제점 및 해결 방법
* 특정 도메인명에 관한 정보가 해당 서버에 캐싱된 이후 책임 서버에서 갱신되었다면 캐시 메모리는 잘못된 정보 유지
* 캐싱 정보는 일정 시간(TTL-Time To Live)이 지나면 자동적으로 삭제

# DNS 자원 레코드(Resource Record)
자원 레코드(RR)
* DNS 서버에 정보가 저장되고 서비스되는 단위

형식
* name, value, type, TTL

유형(type)
* A : name = 호스트명, value = IP주소
* NS : name = 도메인, value=책임 DNS 서버의 호스트명
* CNAME : name= 별칭 호스트명, value=정규호스트명
* MX : name=별칭 메일서버명, value=정규 메일서버명

TTL(Time To Live)
* 해당 자원 레코드가 캐싱될 때 캐시 메모리에 유지되는 시간

# DNS 정보 등록 절차
등록자(도메인 관리자)
* 도메인명과 책임 DNS 서버의 IP주소를 등록기관에 제출
* 웹 서버명과 IP 주소의 A 유형 RR을 책임 DNS 서버에 저장

도메인명 등록기괸(registar)
* 등록 대상 도메인명의 유일성을 검증
* 도메인을 담당하는 책임 DNS 서버에 대한 NS 유형 RR과 A 유형 RR(Resource Record)을 상위 DNS 서버(TLD)에 등록