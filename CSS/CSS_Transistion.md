# Transition
속성을 서서히 변화시키는 속성
property duration timing-function delay에 대해 알아야 한다
timing-fuction과 delay는 생략가능하다

    .box{
        transition: all 1000ms;
    } 
    .box.active{

    }
property는 무엇이 바뀔지 
duration은 몇 초 동안 바뀔지이다
delay는 delay만큼 있다가 바뀐다

## timing-function
바뀌는 방법이다
ease-in ease-out cubic-bezier
ease-in은 천천히 바뀌다 빨리
ease-out은 빠르다가 천천히
cubic-bezier은 사용자가 직접 설정할 수 있다

