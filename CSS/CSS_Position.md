# position이란?
원하는 위치에 자유롭게 이동시키기 위해 사용하는 속성
어떤 type의 position을 사용하고 있는지, 사용하는 요소는 무엇을 기준으로 요소를 이동시키는지 기준점을 생각해야 한다.
position type에 따라 기준점이 많이 달라진다.


## position static
모든 요소의 기본 position type


## position relative 
자기자신이 원래 있던 자리를 기준으로 위, 아래, 좌, 우로 이동한다
원래 있던 위치에 대해서는 기억하고 있다 그래서 레이아웃은 그대로이다


## position absolute
static이 아닌 조상의 position 위치를 기준으로 element의 offset을 적용하고 싶을 때 사용하는 position
float과 비슷하게 display가 block으로 바뀌고 부모가 인식을 하지 못하게 된다, 하지만 inline요소도 인식을 못하게 된다는 차이가 있다
absolute는 기준점을 정할 수 있다, position이 static가 아닌 요소를 기준으로 할 수 있다
주변에 영향을 주지 않는 relative를 기준으로 삼으면 좋다


## position fixed 
fixed를 absolute와 비슷하지만 기준점이 viewport로 고정되어 있다