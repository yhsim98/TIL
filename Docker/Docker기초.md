# Docker 란?
하드웨어 가상화 없는 격리된 환경에서 실행되는 프로세스

## 컨테이너는 VM 인가요?
* VM은 하드웨어를 가상화
    * 소프트웨어로 구현된 하드웨어
* 컨테이너는 하드웨어 가상화가 아니다
    * OS에서 지원하는 기능을 사용한다
    * 격리된 환경에서 실행되는 프로세스

# 우분투 환경에서의 설치
우분투에서 패키지로 직접 설치하는 방법입니다

    sudo apt-get update
    sudo apt-get install docker.io
    sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker

/usr/bin/docker.io 실행파일을 /usr/local/bin/docker 로 