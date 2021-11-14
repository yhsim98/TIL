# SQL Server data types
테이블의 모든 칼럼, 정의된 변수, 파라미터들은 반드시 데이터 타입을 가진다

데이터 타입에는 카테고리들이 있다.

1. Exact numerics : Unicode character strings
2. Approximate numerics : Binary strings
3. Data and time : Other data types
4. Character strings : 


# Exact-number data types
## bigint
8 Bytes를 차지하며 -2^63 ~ 2^63의 범위를 가진다

## int
4 Bytes를 차지하며 -2^31 ~ 2^31 - 1 의 범위를 가진다

## smallint 
2Bytes를 차지하며 -2^15 ~ 2^15 - 1 의 범위를 가진다

## tinyint
1Byte를 차지하며 0 ~ 255 의 범위를 가진다.

# Approximate numerics
float와 real이 있다. 수에 따라 범위와 차지하는 공간이 달라진다.


# Character Strings
## char
* Fixed-length, non-Unicode string data
* n defines the string length and must be a value from 1 through 8000. The storage size is n bytes
* The ISO synonym for char is character

## varchar 
* variable-length, non-Unicode string data
* n defines the string length and can be a value form 1 through 8000. max indicates that the maximum storage size is 2^31 - 1bytes(2 GB)
* The storage size, in bytes, is the value of the actual data entered + 2 bytes.

