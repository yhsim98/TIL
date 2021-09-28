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



