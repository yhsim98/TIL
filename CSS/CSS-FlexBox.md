# Flexbox이란?
Flexible Box module의 약자로 flexbox인터페이스 내의 아이템 간 공간배분과 정렬 기능을 제공하기 위한 1차원 레이아웃 모델이다

## 사용법

    .flexbox{
        display: flex;
        /* flex 명시 */    
        flex-direction: row;
        /* row | row-reverse | column | column-reverse */
        /* 정렬 방향설정*/
        flex-wrap: nowrap | wrap;
        /* 한 줄로 정렬할지 안할지 결정 */
        justify-content: center;
        /* center | flex-end | space-between | space-around ... */
        /* main축으로 정렬 */
        align-items: center;
        /* center | flex-start | flex-end .... */
        /* cross축으로 정렬 */
        align-content: ;
        /* flex-wrap: wrap일경우 의미있다 cross축으로 정렬*/
    }
    
정렬을 하고자 하는 요소를 감싸는 부모에게 display: flex를 주고, 
정렬 방향을 설정하고, 한 줄로 정렬할지 설정하면 된다 


## Axis
Main axis, Cross axis
정렬하고자 하는 방향으로 main축이, 그와 정확하게 수직으로 cross축이 생긴다 
이 축을 기준으로 정렬하게 된다


## order

    .box2{
        order: 2;
    }
    .box1{
        order: 1;
    }

order가 오름차순이 되게 정렬이 된다