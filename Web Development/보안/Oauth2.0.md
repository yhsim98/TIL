# Oauth2.0 이란?
서비스간 인증정보 공유 -> 하나의 인증 서비스로 여러 서비스의 인증을 지원한다.

구글로그인, 네이버로그인 등등 많이 보이는 타 서비스에 인증과정을 위임하는 인증 방식을 Oauth 2.0 인증이라고 한다.



# Oauth2.0 인증과정

![](https://developers.payco.com/static/img/@img_guide.jpg)


![](https://developers.payco.com/static/img/@img_guide2.jpg)


위 사진들은 [페이코 사이트](https://developers.payco.com/guide/development/start)에서 가져온 사진들이다.

1. 사용자의 접근 및 로그인 요청
2. 자사의 서비스 즉 애플리케이션이 인증 서비스를 제공하는 타 서비스에 로그인 요청을 보낸다. 
3. 타 서비스에 사용자가 로그인 등 인증을 완료하면 타 서비스가 Authorization Code를 발급한다.
4. 서비스 등록시 등록한 리다이렉션 URL로 redirect 된다.
    * redirect 때 자사의 서비스는 callback URL을 통해 Authorization Code를 전달받는다.
5. 전달받은 authorization code를 통해 인증 서비스를 제공하는 서비스에 Access Token을 요청, 발급받아 저장, 관리한다.
6. 사용자가 서비스를 요청하면 발급받은 Access Token을 통해 인증 서비스를 제공하는 서비스를 통해 검증한다
7. 문제가 없으면 서비스를 제공한다

위는 임의로 만든것이며 얼마든지 수정될 수 있다.. 그리고 서비스에 따라 조금식 차이를 보이는 것도 같다.

