# UNION
조회한 다수의 SELECT 문의 결과를 하나로 합치고 싶을 때 사용한다.

UNION은 결과를 합칠 때 중복되는 행은 하나만 표시해준다.

단 SELECT 문의 컬럼의 개수가 같아야 한다.

UNION ALL을 사용하면 중복제거를 하지 않는다.



    SELECT * FROM A
    UNION (ALL)
    SELECT * FROM B


