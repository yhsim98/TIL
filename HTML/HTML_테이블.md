# 테이블이란?
데이터를 담은 표를 만들 때 사용하는 것

## table 태그

    <table></table>

테이블을 사용한다고 브라우저에 알려주는 태그

## tr, th 태그
    <thead>
        <tr>
            <th></th>
            <th></th>
        </tr>
    </thead>
    <tr>
        <th></th>
        <td></td>
    <tr>

tr은 행을 정의할 때 사용하고, 
th는 헤더 셀을 정의할 떄 사용된다.
td는 각 셀의 내용을 정의할 때 사용된다.
thead는 헤더 셀이라는 것을 강조할 때 사용한다.


## rowspan

    <td rowspan="2"></td>

한 셀이 여러 열을 사용할 때 사용하는 속성   
아래 열을 입력할 때 해당 열부분은 생략해 줘야함 


## colspan 

    <td colspan=""></td>

rowspan과 같지만 열이 아니라 행


## scope

    <th scope="row || col"></th>

브라우저에게 행에 th인지 열의 th인지 더 많은 정보를 전달해 준다.