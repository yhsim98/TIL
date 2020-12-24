# 미디어 파일이란?
html문서 안에 텍스트가 아닌 어떤 형태의 데이터를 집어넣는 경우

## audio 태그

    <audio src=""></audio>

오디오 파일을 넣을 때 사용하는 태그    
autoplay, controls, loop 등으로 오디오 파일을 컨트롤할 수 있다.


## source태그

    <audio>
        <source src="" type="" />
        <source src="" type="" />
    </audio>

주소를 나타내는 src와 영상의 형식을 나타내는 type을 명시해야 한다.
해당 브라우저에서 type을 지원하지 않는 경우 다른 source의 영상을 출력해 준다.

