# Box


## <strong>Box Model이란?</strong>
position, margin, border, padding, content 순으로 되어있는 모델
html의 태그들은 모두 이 박스 모델 가지고 있다. 
position은 없을 수도 있다.

![Box Model](https://cdn.filepicker.io/api/file/4XNqriWaTAqdsIo7hIWq)



### content

    .box{
        width: 40px;
        height: 40px;
    }

content가 들어 있는 상자 
가로는 width, 세로는 height 


### padding 

    .box{
        padding-top: 20px;
        padding-bottom: 20px;
        padding-left: 20px;
        padding-right: 20px;
        padding: 10px 20px 30px 40px; 
    }

content와 border 사이의 공간을 나타내는 padding


### border

    .box{
        border: 1px /*색상*/ /*스타일*/; 
        border-radius: 4px; //둥근 모서리
        border-radius: 50%; //원형
    }

테두리를 나타내는 Border
만들때 굵기, 스타일, 색상을 명시해야 한다.
border-radius라는 속성이 있는데 둥글게 만들어주는 속성이다.


### margin 

    .box{
        margin-bottom: 40px;
        margin-top: 40px;
        margin: 10px 20px 30px 40px;
    }

요소와 요소사이의 간격을 나타낸다.


### shortend 속기형
top right bottom left순으로 

    padding: 10px 20px 30px 40px;

top과 bottom이 한세트 left와 right가 한세트

    margin : 10px 20px


### box-sizing 요소
box-sizing값은 보통 content-box기준으로 잡게 된다. 그래서 box-sizing을 따로 지정해주지 않으면 박스의 크기를 지정할 때 모든 요소의 합을 고려해서 할 필요가 있게 된다. 따라서 box-sizing을 따로 지정해 주는 것이 좋다.

    *{
        box-sizing: padding;
        box-sizing: border;
    }

'*'은 전체 선택자를 의미한다. 모든 태그를 지칭할 때 사용하면 된다.


## display 속성
요소를 어떻게 보여줄지를 결정하는 속성 태그마다 기본값이 다르며,
none, block, inline, inline-block이 있다.


## <strong>block이란?</strong>
div 태그, p 태그, h 태그, li 태그 등이 이에 해당된다. 

### block의 특징
#### width
기본적으로 가로 영역을 모두 채운다
따로 width를 선언하지 않은 경우, width는 부모의 content-box만큼 할당된다
따로 width를 선언한 경우, 남은 공간은 margin으로 자동으로 채운다
    margin-left: auto;
    margin-right: auto;
    margin: 0 auto;
등으로 margin을 어느 방향으로 할지 지정할 수 있다.

#### height
부모의 height에 별도의 height값을 지정하지 않은 경우, 자식 요소의 height의 합이 부모의 height가 된다. 



## <strong>Inline이란?</strong>
span 태그, b 태그, i 태그, a 태그 등이 이에 해당된다. 


### inline 특징
inline박스의 경우 
block과 달리 줄바꿈이 되지 않고, 위 아래줄에 영향을 주는 
width, height, padding-top, padding-bottom, border-top, border-bottom, margin-top, margin-bottom을 쓸 수 없다 
볼드, 이탤릭, 색상 등 글자나 문장에 효과를 주기 위해 주로 사용된다
문서에서 특정 부분에 색상을 입힌다고 다음에 나오는 글이 줄바꿈 되지 않듯이 inline요소 뒤에 나오는 태그 또한 줄바꿈 되지 않고 바로 오른쪽에 표시된다.


## <strong>inline-block이란?</strong>
inline이 세로 배치가 힘들고, block이 가로 배치가 힘든 점을 보완한 display속성

    display: inline-block;

display를 inline-block으로 설정해 주면 따로 줄바꿈이 되지 않고 top, bottom을 설정도 가능하다 



## <strong>float이란?</strong>
block요소들을 가로배치하기 위해 사용하는 요소


### flaot 특징
float를 사용하면 부모는 빈공간으로 인식하게 된다
height를 계산할 경우 float를 사용하면 없는 것으로 취급되어 부모의 height를 계산할 때도 제외하고 계산한다

float를 사용하면 inline이던 inline block이던 block으로 바뀌게 된다.

이렇게 block로 바뀌어도 block의 width의 특징을 따르지는 않는다
남는 공간을 자동으로 margin으로 채우지 않는다

텍스트나 이미지같은 inline 요소들은 float를 인식해서 정렬되어 레이아웃이 박살난다


### overflow: hidden;
이 속성을 주면 부모가 인식할 수 있게 된다. 


### clearfix
float로 인하여 망가진 layout을 고치기 위한 속성
left, right, both로 나뉘는데
left는 float-left를 무시, right는 float-right를 무시, both는 둘 다 무시해준다 
이 속성을 주면 float하지 않은 요소가 float을 인식할 수 있게 된다
display: block인 것만 사용 가능하다


### Pseudo Element
실제 html에는 존재하지 않는 가상요소
각 요소당 2개씩 만들 수 있다
content는 반드시 작성해야 한다

    .pseudo::before{
        content: "";
    }
    .pseudo::after{
        content: "";
    }

이 요소에 clear속성을 줘서 float에서 무너진 layout을 고치면 된다



