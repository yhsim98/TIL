# Nginx
트래픽이 많은 웹사이트의 확장성을 위해 설계된 비동기 이벤트 기반구조의 웹서버 소프트웨어이다.

러시아 프로그래머 이고르 시쇼브가 Apache의 C10K Problem(하나의 웹서버에 10000개의 클라이언트의 접속을 동시에 다룰 수 있는 기술적인 문제)를 해결하기 위해 만든 Event-driven 구조의 HTTP, Reverse Proxy, IMAP/POP PROXY server를 제공하는 오픈소스 서버 프로그램이다.

# Apache vs Nginx
기존에 많이 사용하던 Apache와 Nginx를 비교해보자

Apache
* 스레드/프로세스 기반 구조로 요청 하나당 스레드 하나가 처리하는 구조
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
