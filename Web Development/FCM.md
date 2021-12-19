# FCM(Firebase Cloud Messaging)
무료로 메시지를 안정적으로 전송할 수 있는 교차 플랫폼 메시징 솔루션이다.

FCM을 사용하면 IOS, Android, Web 등 각각 환경별로 개발할 필요없이 한번에 메시지 발송을 처리할 수 있다.

현재 구글에서 지원하고 있다.

# 주요 기능
메시지 타입(메시지 전송), 타겟팅, 클라이언트 앱에서 전송 3가지로 구분된다.

## FCM message 타입
* Notification Message(알림 메시지)
    * FCM SDK에서 자동으로 처리한다
    * background notifications
    * foreground notifications
        * onMessage : 정상적인 상황에 message 받을 때
        * onLaunch : app이 완전 종료되었다 실행될 때
        * onResume : app이 background에 있다가 foreground로 올 때
* Data message(데이터 메시지)
    * key-value pair로 data를 device에 보낼 수 있음. notification으로 동작하지 않는다
    * client에서 메시지 처리를 담당한다

클라이언트 앱을 대신하여 FCM이 알림 표시 작업을 처리하게 하려면 알림 메시지를 사용하면 된다. 클라이언트 앱에서 메시지를 직접 처리하려면 데이터 메시지를 사용하면 된다.

보통은 알림 및 데이터 메시지를 같이 혼용하여 사용한다. 휴대폰 푸시 알림은 알림 메시지, 알림을 클릭하였을 때 앱 내 특정 페이지로 이동이나, 어떠한 액션은 데이터 메시지를 통하서 이뤄진다.


  
## message Targeting
단일 기기, 기기 그룹, 주제를 구독한 기기 등 3가지 방식으로 클라이언트 앱에 메시지를 배포할 수 있다.

* 단일 기기
    * 대상 1개
    * 하나의 기기(앱 기준)
* 기기 그룹
    * 대상 20개
    * 알림 키에 허용되는 그룹
* 주제 구독 
    * 대상 1000개 
    * 등록 토큰에 구독된 기기

## 클라이언트 앱에서 매세지 전송
기기에서 다시 서버로 확인, 채팅, 기타 메시지를 보낼 수 있다.

기기에서 Firebase Connections Server로 메시지를 전송한다.

예를 들어 기기에서 채팅 및 특정 기타 메시지를 보낸다.


# 작동 원리
크게 송신자, FCM, 수신자로 구분된다.

송신자는 주로 앱 서버나 백오피스 정도의 GUI 콘솔이 되고, 수신자는 우리가 흔히 쓰는 ios나 android 기기이다.

![](https://firebase.google.com/docs/cloud-messaging/images/diagram-FCM.png?hl=ko)

서버가 FCM Server로 메시지 요청을 보내고, FCM 서버에서는 다시 클라이언트 기기 앱에 메시지를 보낸다.

# 서버에서 FCM Server로 전송
크게 3가지 방법이 있다.

1. Firebase Admin SDK
2. HTTP V1 API 
3. 기존 HTTP API

## Firebase Admin SDK
자체 백엔드 서비스를 FCM과 통합할 수 있다.

### 의존성 추가
우선 SDK를 설치해야 한다. 
java maven의 경우 pom.xml에 
    
    <dependency>
        <groupId>com.google.firebase</groupId>
        <artifactId>firebase-admin</artifactId>
        <version>8.1.0</version>
    </dependency>

의존성을 추가하면 된다.

### SDK 초기화
서비스 계정을 인증하고 Firebase 서비스에 엑세스하도록 승인하려면 JSON 형식의 비공개 키 파일을 가지고 있어야 한다.

먼저 비공개 키 파일을 생성하는 방법은
1. Firebase Console 설정 -> 서비스 계정
2. 새 비공개 키 생성 
3. 키가 들어있는 JSON 파일을 저장이다.

![](https://t1.daumcdn.net/cfile/tistory/99C34E355C5D2D761B)

애플리케이션에 사용자 인증 정보를 제공할 수 있는 방법이 3가지 있다.
* 코드에서 서비스 계정 키 경로를 명시적 지정
* GCLOUD_PROJECT 환경변수 설정
* 서비스 계정 사용자 인증 정보로 SDK를 초기화 
이다.


# 디바이스 토큰