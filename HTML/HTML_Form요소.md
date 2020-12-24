## Form요소 
사용자로부터 input을 받기 위한 태그이다. 
Syntax Alert을 주의해야 한다.

## form 태그

    <form action="/*API address*/" method="/*get || post*/"></form>

 브라우저에게 form요소를 사용한다고 전달하는 태그 
 action과 method속성을 반드시 같이 전달해 주어야 한다.
 action은 동작을 알려주는 특성이다.     


 ## input 태그

    <input type=""/>

type속성을 반드시 명시해주어야 한다.
type이란 입력의 종류를 나타내는 속성이다.   
이 외에도 placeholder 등 다양한 속성이 있다. 


## label 태그

    <label for="id"></label>
    <input type="" id="id"/>

폼 양식에 이름을 붙여주는 태그 
for속성을 통해 어떤 요소를 가리키는지 명시해야 한다.


## radio, checkbox

    <input type="radio" name="name1" value="sub" id="id1"/>
    <label for="id1"></label>
    <input type="radio" name="name1" value="unsub" id="id2"/>
    <label for="id2"></label>
    
radio는 같은 name을 가지는 그룹 내에서 하나만 선택할 수 있게 하는 type이다.
radio를 쓸 때는 name과 value를 반드시 사용해야 한다.
name은 같은 그룹이란 것을 알려주고,
value는 서버에 전달할 때 그룹 내에서 어떤 항목이 선택되었는지 구분해 준다. 
checkbox는 다중 선택을 할 수 있다. 


## select, option 태그

    <select name="">
        <option value=""></option>
        <option value=""></option>
    </select>

여러 option중 하나를 선택하는 경우 select태그를 사용


## textarea 태그 

    <textarea></textarea>

많은 입력을 받을 때 사용하는 태그 


## button 태그

    <button type=""></button>

type을 꼭 명시해야 한다.
type에는 button, submit, reset
submit은 폼을 서버에 전달하는 경우,
reset은 작성한 폼을 초기화하는 경우 사용한다. 
