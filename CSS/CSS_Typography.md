# Typography이란?
typography는 읽기 용이하도록 텍스트 디자인을 꾸며주는 것을 말한다
폰트 스타일 속성과 텍스트 레이아웃 속성으로 나뉜다


## 폰트 스타일 속성
폰트사이즈에는 px, em, rem이 있다 
px는 절대적인 수치이고 em과 rem은 상대적이다
em 단위는 상위 요소 크기의 배수로 크기를 정한다
rem은 root em의 약자로 여기서 root는 html을 의미한다 html태그에 있는 텍스트 크기의 배수로 크기를 정한다

### line-height
줄간격인을 표시해 준다
표시하는 방법도 여러가지가 있는데 보통 em으로 상대적으로 정한다 em은 생략해 준다
line-height값에 상관없이 글자는 줄간격의 가운데에 항상 위치하게 된다

### letter-spacing
글자사이의 자간을 뜻한다 
주로 px과 rm을 사용한다 '-'면 줄어들고 '+'면 늘어난다

### font-family
사용하고자 하는 서채를 나열하면 된다

### font-weight
폰트의 굵기를 뜻한다 
100의 배수를 사용하면 된다 
/* 100 | 200 | 300 | ..... */

### color
사용하고 싶은 색상을 적으면 된다 
    color: rgba(색상, 색상, 색상, 투명도);
    color: rgb( , , );

### text-align
텍스트를 정렬할 때 사용한다
    text-align: left | right | center;

### text-indent
들여쓰기를 할 때 사용한다 
    text-indet: px;

### text-transform
알파벳의 대소문자와 관련된 속성이다
    text-transform: capitalize | uppercase | lowercase;
capitalize는 모든 단어의 가장 앞에 있는 알파벳만 대문자로 바꿔준다 

### text-decoration
텍스트의 밑줄등과 관련된 속성이다 

    text-decoration: none | underline | line-through | overrline;

### font-style
텍스트의 기울임과 관련된 속성이다


    font-style: normal | italic | oblique;


## Webfont
만약 웹사이트의 제작에 사용한 폰트가 사용자의 컴퓨터에 없는 경우를 대비할 때 사용된다
html에 직접 import하거나 css에 import할수 도 있다

    @font-face{
        font-family: ""; //사용할 서체의 이름을 정하기
        font-style: ; 
        /* italic인지 normal인지등 어떤 스타일의 폰트인지 명시*/
        font-weight: ;
        src: url('/* 경로 */') format('/* 폰트명 */');
    }



