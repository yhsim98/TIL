# Nginx
비동기 이벤트 기반구조의 웹 서버

리버스 프록시 server, HTTP Server기능 제공

HTTP server
* 정적 파일 처리

리버스 프록시
* 실제 포트 숨김, 리다이렉트 지원
* 로드벨런싱 가능, 보안적 이점

## 리버스 프록시
요청을 중계하는 역할을 하는 것이 프록시 서버

* 포워드 프록시
    * 클라이언트에서 서버로 요청할 때 프록시 서버를 거쳐서 요청
    * 서버입장에서는 프록시 서버의 IP로 요청이 들어오기 때문에 클라이언트가 누구인지 알 수 없다
        * 클라이언트를 감출 수 있다
        * 기업 사내망에서 주로 사용
    * 캐싱, IP우회, 제한 등 설정 가능
* 리버스 프록시 
    * 서버앞에 위치하여 클라이언트는 서버를 요청할 때 리버스 프록시를 호출하고, 
    * 리버스 프록시가 서버로부터 응답을 전달받아 클라이언트에게 다시 전송
    * 애플리케이션 서버를 감출 수 있다
        * nginx, apache 등
    * 로드밸런싱, 서버에 직접 접근하는 것을 막아 보안

트래픽이 많은 웹사이트의 확장성을 위해 설계된 비동기 이벤트 기반구조의 웹서버 소프트웨어이다.

러시아 프로그래머 이고르 시쇼브가 Apache의 C10K Problem(하나의 웹서버에 10000개의 클라이언트의 접속을 동시에 다룰 수 있는 기술적인 문제)를 해결하기 위해 만든 Event-driven 구조의 HTTP, Reverse Proxy, IMAP/POP PROXY server를 제공하는 오픈소스 서버 프로그램이다.

정적 파일을 처리하는 HTTP Server의 기능을 수행해주며, Nginx서버를 프록시서버 앞단에 놓아 실제포트를 숨기고 nginx의 80포트를 통해서 프록시하는 Reverse proxy server의 기능도 제공한다. 해당 방법으로 버퍼 오버플로우 방지등 보안적으로 이점이 있고 로드벨런싱(여러 서버로 부하 분산)도 가능하다.

그 외에도 Mail proxy server, Generic TCP/UDP proxy server 등의 기능이 있다.

# Apache vs Nginx
기존에 많이 사용하던 Apache와 Nginx를 비교해보자

Apache
* thread/process 기반 구조로 요청 하나당 스레드 하나가 처리하는 구조
* 사용자가 많으면 많은 스레드 생성, 메모리 및 CPU 낭비가 심함
* 하나의 스레드 : 하나의 클라이언트

Nginx
* 비동기 Event-Driven 기반 구조
* 다수의 연결을 효과적으로 처리가능
* 대부분의 코어 모듈이 Apache보다 적은 리소스로 더 빠르게 동작가능
* 더 작은 스레드로 클라이언트의 요청들을 처리가능

Apache와 Nginx의 가장 큰 차이는 결국 Thread 방식과 Event-driven방식의 차이이다.

![쓰레드 방식](https://user-images.githubusercontent.com/42582516/104517420-4bd05080-5639-11eb-92a5-dc3f78cc5891.png)

![Event-driven 방식](https://mblogthumb-phinf.pstatic.net/MjAxNzAzMjZfMTM3/MDAxNDkwNDk1NjMxNzgy.OHZ33nerX_6Hc92Mg_xjr51acwwi1P_mq3SIl7Cuhisg.niRsQQVM5CwGpXKcdOxl3bkNsmfBkqGV1ajcBpV6CvQg.GIF.jhc9639/mighttpd_e02.gif.gif?type=w800)

스레드 방식은 하나의 커넥션당 하나의 스레드를 사용한다. 하지만 Event-driven 방식은 여러 커넥션을 모두 Event-Handler를 통해 비동기 방식으로 순차적으로 처리하게 된다.

Event-driven 방식으로 처리하게 된다면 더 적은 자원으로 더 좋은 성능을 낼 수 있다.



# nginx 기본 설정
nginx.conf 파일을 통해 설정할 수 있다.

기본 경로는 /etc/ningx/

2. HTTP 블록
웹 서버에 대한 동작을 설정하는 영역으로, server 블록과 location 블록의 루트 블록이다.

3. server 블록
하나의 웹사이트를 선언하는 데 사용된다. 가상 호스팅(Virtual Host)의 개념이다.

4. location 블록
server 블록 내에서 특정 URL을 처리하는 방법을 정의한다

5. events 
주로 네트워크 동작에 관련된 설정하는 영역으로, 이벤트 모율을 사용한다.

    # worker 프로세스를 실행할 사용자 설정
    # - 이 사용자에 따라 권한이 달라질 수 있다.
    user  nginx;
    # 실행할 worker 프로세스 설정
    # - 서버에 장착되어 있는 코어 수 만큼 할당하는 것이 보통, 더 높게도 설정 가능
    worker_processes  1;

    # 오류 로그를 남길 파일 경로 지정   
    error_log  /var/log/nginx/error.log warn;
    # NGINX 마스터 프로세스 ID 를 저장할 파일 경로 지정
    pid        /var/run/nginx.pid;


    # 접속 처리에 관한 설정을 한다.
    events {
        # 워커 프로레스 한 개당 동시 접속 수 지정 (512 혹은 1024 를 기준으로 지정)
        worker_connections  1024;
    }

    # 웹, 프록시 관련 서버 설정
    http {
        # mime.types 파일을 읽어들인다.
        include       /etc/nginx/mime.types;
        # MIME 타입 설정
        default_type  application/octet-stream;

        # 엑세스 로그 형식 지정
        log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                        '$status $body_bytes_sent "$http_referer" '
                        '"$http_user_agent" "$http_x_forwarded_for"';

        # 엑세스 로그를 남길 파일 경로 지정
        access_log  /var/log/nginx/access.log  main;

        # sendfile api 를 사용할지 말지 결정
        sendfile        on;
        #tcp_nopush     on;

        # 접속시 커넥션을 몇 초동안 유지할지에 대한 설정
        keepalive_timeout  65;

        # (추가) nginx 버전을 숨길 수 있다. (보통 아래를 사용해서 숨기는게 일반적)
        server_tokens off;

        #gzip  on;

        # /etc/nginx/conf.d 디렉토리 아래 있는 .conf 파일을 모두 읽어 들임
        include /etc/nginx/conf.d/*.conf;
    }
    [출처 https://kscory.com/dev/nginx/install]



# nginx 튜닝하기
https://couplewith.tistory.com/entry/%EA%BF%80%ED%8C%81-%EA%B3%A0%EC%84%B1%EB%8A%A5-Nginx%EB%A5%BC%EC%9C%84%ED%95%9C-%ED%8A%9C%EB%8B%9D-1-%EB%94%94%EC%8A%A4%ED%81%AC%EC%9D%98-IO-%EB%B3%91%EB%AA%A9-%EC%A4%84%EC%9D%B4%EA%B8%B0

nginx 튜닝을 통해 성능도 늘리고 보안도 강화할 수 있다.