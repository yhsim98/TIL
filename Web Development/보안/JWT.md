# JSON Web Token
웹 표준(RFC 7519)으로서 두 개체에서 JSON 객체를 사용하여 가볍고 자가수용적인(self-contained) 방식으로 정보를 안정성 있게 전달해 준다.

## 수많은 프로그래밍 언어에서 지원된다
웹 표준이라 대부분의 주류 프로그래밍 언어에서 지원된다.

## 자가수용적이다
JWT는 필요한 모든 정보를 자체적으로 지니고 있다. JWT 시스템에서 발급된 토큰은, 토큰에 대한 기본정보, 전달 할 정보 그리고 토큰이 검증됐다는 것을 증명해주는 signature를 포함하고 있다.

## 쉽게 전달 될 수 있다
JWT는 자가수용적임으로 두 개체 사이에서 손쉽게 전달 될 수 있다. 웹 서버의 경우 HTTP의 헤더에 넣어 전달할 수 있고, URL의 파라미터로 전달 할 수도 있다.

# JWT 사용처
* 회원인증
    * 토큰 기반 인증 기술에 사용한다
* 정보 교류
    * 정보가 중간에 조작되지 않았는지 검증할 수 있기 때문에 정보를 안정성 있게 교환하기 좋다

# JWT 구조
JWT는 헤더(header).내용(payload).서명(signature)로 구성되어 있다. 

![](https://velopert.com/wp-content/uploads/2016/12/jwt.png)

## 헤더(Header)
Header는 두 가지 정보를 지니고 있다.

* typ 
    * 토큰의 타입을 지정한다. 여기서는 JWT이다
* alg
    * signature를 생성할 때 사용할 해싱 알고리즘을 지정한다. 보통 HMAC, RSA등이 사용된다. 


    {
        "typ" : "JWT",
        "alg" : "HS256"
    }

이렇게 두 가지 정보를 base64로 인코딩하여 헤더를 생성한다.


## 내용(payload)
Payload 부분에는 토큰에 담을 정보가 들어있다. 여기에 담는 정보의 한 조각??을 클레임(claim) 이라고 부르고, 이는 name/value의 한 쌍으로 이뤄져 있다. 토큰에는 여러 개의 클레임 들을 넣을 수 있다.

클레임의 종류는 다음과 같이 크게 세 분류로 나눠져 있다.
* 등록된(registered) 클레임
* 공개(public) 클레임
* 비공개(private) 클레임

### 등록된(registerd) 클레임
등록된 클레임들은 서비스에서 필요한 정보들이 아닌, 토큰에 대한 정보들을 담기위하여 이름이 이미 정해진 클레임들이다. 등록된 클레임의 사용은 모두 선택적(optional)이며, 이에 포함된 클레임 이름들은 다음과 같다.

* iss
    * 토큰 발급자(issuser)
* sub 
    * 토큰 제목(subject)
* aud
    * 토큰 대상자(audience)
* exp
    * 토큰의 만료시간(expiration)
    * 시간은 NumericDate 형식으로 되어있어야 하며(ex: 1480849147370) 언제나 현재 시간보다 이후로 설정되어있어야 한다
* nbf
    * Not Before를 의미하며, 토큰의 활성 날짜와 비슷한 개념이다. 여기에도 NumericDate 형식으로 날짜를 지정하며, 이 날짜가 지나기 전까지는 토큰이 처리되지 않는다
* iat
    * 토큰이 발급된 시간(issued at), 이 값을 사용하여 토큰의 age가 얼마나 되었는지 판단할 수 있다
* jti 
    * JWT의 고유 식별자로서, 주로 중복적인 처리를 방지하기 위하여 사용된다. 일회용 토큰에 사용하면 유용하다

### 공개(public) 클레임
공개 클레임들은 충돌이 방지된(collision-resistant)이름을 가지고 있어야 한다. 충돌을 방지하기 위해서는, 클레임 이름을 URI 형식으로 짓는다.

### 비공개(private)클레임
등록된 클레임도 아니고, 공개된 클레임들도 아니다. 양 측간에 (보통 클라이언트 <-> 서버) 협의하에 사용되는 클레임 이름들이다. 공개 클레임과는 달리 이름이 중복되어 충돌이 될 수 있으니 사용할 때에 유의해야 한다.

### 주의사항
JWT 토큰은 가끔 URL의 파라미터로 전달 될 때도 있는데 '='문자는 url-safe 하지 않음으로 제거되어야 한다.


### 서명(signature)
헤더의 인코딩값과 정보의 인코딩값을 함친 후 비밀키로 해쉬를 해서 생성한다.
 
    HMACSHA256(header.payload, secret)

이렇게 만든 해시를, base64 형태로 나타내면 된다.(문자열을 인코딩하는 것이 아닌 hex -> base64 인코딩을 해야 한다)

#### 해싱
해시 함수란 임의의 길이의 데이터를 고정된 길이의 데이터로 매핑하는 함수이다. 

보통 송수신자간 메시지를 주고받을 때, 메시지가 변조되었는지 확인할 필요가 있는데 이때 사용하는 해시 방식이 MAC이다.

그중 해싱과 공유키를 사용한 MAC 기술이 바로 **HMAC**이다.

원본 메시지가 변하면 그 해시값도 변하는 해싱의 특징을 활용하여 메시지 변조 여부를 확인하여 무결성과 기밀성을 제공한다.

추가로 공부하고 내용을 추가하자






# spring에서의 반응
서버로 JWT을 보낼 경우 HttpServletRequest의 header에 Authorization 이라는 키로 보내게 된다.




