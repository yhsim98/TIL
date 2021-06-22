# 클라이언트에서 서버로 데이터 전송

## 데이터 전달 방식
* 쿼리 파라미터를 통한 데이터 전송
    * GET
    * 정렬 필터(검색)
* 메시지 바디를 통한 데이터 전송
    * POST, PUT, PATCH
    * 회원가입, 상품 주문, 리소스 등록, 리소스 변경

## 4가지 상황
### 정적 데이터 조회
쿼리 파라미터를 사용하지 않는다.

* 이미지, 정적 텍스트 문서
* 조회는 GET 사용
* 정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능

### 동적 데이터 조회
쿼리 파라미터 사용.  
//www.google.com/search?q=hello&hl=ko 등등
 
* 주로 검색, 게시판 목록에서 정렬 필터(검색어)
* 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
* 조회는 GET 사용
* GET은 쿼리 파라미터 사용해서 데이터 전달

### HTML Form 데이터 전송
POST전송 - 저장  
form태그로 데이터 전송  

submit하게 되면 http 메시지가 생긴다
* content-type : application/x-www-form-urlencoded
* body내용은 key=value 형식이다
    * key=value&key=value
* 만약 post를 GET으로 바꾸면 url경로에 query 파라미터로 body내용을 넣게 된다

* multipart form data로 여러 형식의 데이터를 서버로 전송하는 것도 가능하다


