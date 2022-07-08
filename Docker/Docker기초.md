# Docker 란?
컨테이너 기반의 오픈소스 가상화 플랫폼으로 

하드웨어 가상화 없는 격리된 환경에서 프로세스를 실행시킵니다. 

도커 파일을 작성해서 빌드를 통해 이미지를 생성하고 구동시키면 도커 컨테이너 위에서 실행이 된다.

운영체제에 독립적이다.

## 리눅스 컨테이너
리눅스 상에서 여러 개의 격리된 리눅스 시스템을 동작시키기 위한 운영체제 가상화 기술

리눅스 커널(Kernel) 위에 Libvirt가 돌아가며 그 위에 컨테이너가 올라가는 방식

다양한 프로그램, 실행환경을 컨테이너로 추상화하고 동일한 인터페이스를 제공하여 프로그램의 배포 및 관리를 단순하게 해줍니다.

DB, 백엔드 서버, 메시지 큐등 어떤 프로그램도 컨테이너로 추상화할 수 있고 어디서든 실행할 수 있습니다.

## 컨테이너 vs VM
* 가상머신은 OS를 가상화하여 사용하는 방식
* 느리고 무겁다
* 그래서 CPU의 가상화 기술을 이용한 KVM(Kernel-based virutal machine)과 반가상화(Paravirtualization)방식의 Xen이 등장

## Linux와 Docker
Linux에는 cgroups, namespaces, netlinke 등 프로세스를 실행시킬 때 해당 프로세스를 다른 환경으로 격리시킬 수 있는 리눅스 커널이 있다.

Docker는 해당 커널들을 이용하여 격리된 프로세스.

## 이미지
* 특정 프로세스를 실행하기 위한 환경
* 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있다
    * 우분투 이미지는 우분투 실행을 윟나 모든 파일을 가지고 있다
    * gitlab 이미지는 centos를 기반으로 ruby, go, database, redis 등을 가지고 있다
* Immutable 하다
* 컨테이너를 실행하기 위한 정말 모든 정보를 가지고 있기 때문에 의존성 파일을 컴파일하고 설치할 필요가 없다
* 프로세스가 실행되는 환경도 결국 파일들의 집합
* 계층 구조를 가진다

### Immutable Infrastructure
특정한 소프트웨어가 자주 수정되는 경우 문제가 발생할 수 있다. 그래서 그냥 **서버를 구축한 이후에는 변경이나 업데이트를 할 수 없도록 하고**, 문제 시 그냥 삭제해버리는 것

## 이미지 계층 구조
![](https://subicura.com/generated/assets/article_images/2017-01-19-docker-guide-for-beginners-1/image-layer-1000-d301d8998.webp)

* 도커 이미지는 용량이 크다
    * 컨테이너 실행에 필요한 모든 정보를 가지고 있기 때문
    * 그래서 약간에 변경에 이미지 전체를 다시 받아야 한다면 비효율적
* 그래서 도커는 계층(layer)라는 개념을 사용
* 이미지는 여러 읽기 전용 레이어로 구성되있다
    * 그래서 파일이 추가되거나 변경되면 새로운 레이어가 생성

ubuntu가 `A + B + C`의 집합이라면, 우분투 이미지를 베이스로 만든 nginx는 `A + B + C + nginx` 여기서 nginx에 변화가 있다면 `nginx`만 새로 받으면 된다

## Client-Server 구조
* 도커는 client-server 구조를 가진다
* server는 client로부터 rest api 를 통해 호출되기 때문에 다른 기기에 있어도 된다
    * 물론 같은 기기에 있어도 됨
* client가 docker 명령어를 api 방식으로 번역하여 호출하기 때문에 의식하지 못할 수 있음
* docker client에서 명령을 보내면 Docker Host로 명령을 보내고 Docker daemon은 Docker Hub의 Registry에서 이미지를 가져온 뒤 Docker daemon이 바라보는 위치에 이미지들이 저장되며 이 이미지를 통해 컨테이너가 실행됨

### docker host
* docer server
* docker daemon을 가지고 실제로 image를 저장하고 container를 돌리는 머신
* 내부에 있는 docker daemon이 클라이언트로부터 명령을 받아 이를 직접 실행시킨다
* 호출하는 client와 server를 통틀어 `docker engine`이라 한다

## 컨테이너 삭제
* 도커에서 업데이트를 위해서 등의 이유로 컨테이널르 삭제하며녀 컨테이너에서 생성된 파일이 사라집니다
    * mysql의 데이터, 받은 이미지 파일 등등 전부 날아감
* 그래서 외부 스토리지(s3)을 사용하거나
* 호스트의 디렉토리를 마운트해서 사용해야 합니다
    * 마운트하면 생성되는 데이터가 호스트의 디렉토리에 저장된다
    * 컨테이너가 삭제되도 데이터가 사라지지 않는다

# Docker Compose
Docker의 복잡한 설정을 쉽게 관리하기 위해 등장한 툴

YAML 방식의 설정파일을 이용하게 된다

# 도커 명령어
## 도커 설치
``curl -fsSL https://get.docker.com/ | sudo sh``

``sudo usermod -aG docker your-user # your-user 사용자에게 권한주기``




## 출저
https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html