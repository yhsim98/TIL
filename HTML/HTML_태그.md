# 태그란?
태그는 .....
태그는 꺽쇠와 속성으로 이루어져 있다. 

## 헤드 태그 

    <h1></h1>
    .
    .
    .
    <h6></h6>   

제목부분을 나타내 준다. 
어떤 내용에 대해 핵심을 말하고 있는 내용을 헤드 태그로 감싸주면 된다. 

## 문단 태그

    <p></p>

 내용을 나타내는 태그이다.    


 ## 강조 태그

    <em></em>
    <strong></strong>

강조를 나타낸 태그로 강조하는 내용을 강조 태그로 감싸면 된다.
em은 <em>이텔릭체</em>로 strong은 <strong>볼드체</strong>로 강조해 주지만 중요한 것은 브라우져에게 이 내용을 강조한다는 것을 전달하는 것이기 때문에 둘 중 아무거나 쓰면 된다.    

## 줄바꿈 태그

    <br/> 


## Anchor(링크) 태그     

    <a href="**"></a> //웹 주소
    <a herf="#hello"> //페이지 내 이동
    <section id="hello">
    <a href="mailto:2008qwe123@gmail.com"> //메일쓰기 기능
    <a href="tel:/*전화번호*/"> //모바일앱에서 자동으로 전화걸기
     <a href="**"target="_blank"></a> //새로운 창에서 열기
    
href는 hypertext reference의 약자로 주소를 나타내 주는 html의 속성이다.    
다른 패이지로 이동도 가능한고 같은 페이지내 이동도 가능하다. 
새로 페이지를 열어서 이동하려 하면 target속성을 이용하면 된다.
그 외에 메일쓰기나 모바일에서 전화로 연결등의 기능이 있다.


## 이미지 태그

    <img src="*" alt="*"/>

img 태그에서 src는 이미지의 경로를 나타내는 속성이고 alt는 대체 텍스트로 이미지가 랜더링되지 못할 때 나타날 문자열을 지정하는 속성이다.    


## 리스트 태그

    <ul></ul> //unorderd list, 순서가 중요하지 않은 리스트
    <ol></ol> //orderd list, 순서가 중요한 리스트 

리스트 태그이다.  자식태그로는 li만 가능하다.     


## 정의 리스트 태그

    <dl>
        <dt>사과</dt> //key
        <dt>apple</dt> //dt를 연속해서 쓰면 같은 용어라는 느낌이다?
        <dd>****</dd> //value1
        <dd>****</dd> //value2
    </dl>

용어를 정의할때 사용하는 태그 
dt는 용어를 나타내는 태그이고 dd는 정의를 나타내는 태그이다.  
dl의 자식은 div, dt, dd만 가능하다  


## 인용 태그

    <blockquote><cite></cite></blockquote> //텍스트로 표현
    <blockquote cite="https://***"></blockquote> //주소를 명시 

 안에 있는 내용이 인용한 내용이란 것을 브라우저에 전달한다.   
 cite를 이용하여 출처를 표시할 수 있다.


 ## div, span 태그

    <div></div>
    <span></span>

요소를 묶어주는 태그로써 아무런 의미가 없다.
div는 다양한 것들을 묶어주는 경우,
span는 작은 텍스트의 일부분을 묶어주는 경우 사용한다.    


## abbr 태그

    <abbr title="">/**/</abbr>

약어를 나타내 주는 태그이다. 


## address 태그

## pre , code태그
 
    <pre>
        <code></code>
    </pre>

Enter, Tab, Space를 전부 활용할 수 있게 해주는 태그이다.  
code를 작성할 때 사용하기 좋은 태그이다.