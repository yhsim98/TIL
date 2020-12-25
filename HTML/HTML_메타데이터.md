## 메타데이터란?

HTML의 head요소에는 데이터를 설명하는 데이터인 메타데이터가 들어가게 된다. 
이러한 head의 내용은 페이지에 표지되지는 않는다. 


## name, content 속성

    <meta name="" content="">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="author" content="yhsim">

메타데이터에는 title등에 들어가기 애매한 것을 넣어주게 된다. 
메타태그에 반드시 작성해야할 속성이다. 
name은 메타데이터의 종류를 넣어주고 
content에는 해당되는 내용을 넣어준다. 
name에 들어가는 것중 viewport라는 것이 있는데 기기의 디스플레이 크기에 맞게 반응형으로 웹 페이지 사이즈를 조절할 수 있게 해준다.
content는 사용자의 디바이스 가로 크기로 맞추라는 것이다.
이 외에도 다양한 것이 들어간다.

## title 태그

    <title>/*제목*/</title>

페이지의 탭에 나타나는 제목이다.  
title을 잘쓰는 것은 검색 최적화에 매우 중요하다. 
그 방법은  
1. 단순히 키워드를 나열하지 않기
2. 페이지마다 그에 맞게 변경
3. 길고 친절하고 함축적으로 하는 것이 좋다


## link 태그

    <link rel="" href="">

css 스타일시트를 첨부하는 태그이다.
link를 입력한 다음 tab키를 누르면 자동으로 들어간다.


# style 태그

    <style></style>

html문서에서 css파일을 작성할 때 필요한 태그이다.


# script 태그

    <script src=""></script>

javaScript를 연결하는 태그이다.
얘는 head가 아닌 body에 선언하게 된다. 
다른 태그는 시간이 걸릴경우 다른 태그를 실행하게 되는데 script는 전부 로딩될때까지 기다리게 됨으로 head에 넣으면 랜더에 굉장히 비효율적이다. 따라서 body중에도 가장 끝에 넣어주는 것이 좋다.    

