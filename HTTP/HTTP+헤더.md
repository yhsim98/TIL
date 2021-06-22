# HTTP 헤더
요청이나 응답에서 전달할 실제 데이터를 표현(representation)이라고 한다. 

Representation은 representation Metadata + Representation Data이다.

HTTP헤더는 representation metadata을 포함하여 representation data를 해석할 수 있는 정보를 제공한다
* 데이터 유형(html, json), 데이터 길이, 압축 정보 등등


# 표현(Representation)
표현에 대한 여러가지 정보가 있다. 이런 정보를 표현 헤더에 저장한다.

* Content-Type : 표현 데이터 형식
    * text/html; charset=utf-8
    * application/json
    * image/png
* Content-Encoding : 표현 데이터 압축 방식
    * 표현 데이터를 압축하기 위해 사용
    * 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
    * 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
    * gzip, deflate, identity 등등
* Content-Language : 표현 데이터의 자연 언어
* Content-Length : 표현 데이터의 길이
    * 바이트 단위


