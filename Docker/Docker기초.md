# Docker 란?
리눅스 컨테이너 기술을 이용한, 프로세스를 격리하여 사용하는 기능

가상머신과 달리 운영체제를 설치할 필요가 없어 가볍고 빠르다.

# 우분투 환경에서의 설치
우분투에서 패키지로 직접 설치하는 방법입니다

    sudo apt-get update
    sudo apt-get install docker.io
    sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker

/usr/bin/docker.io 실행파일을 /usr/local/bin/docker 로 