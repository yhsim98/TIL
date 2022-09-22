# Node.js

- Ndoe.js에서 함수는 일급 객체, 함수의 인수로 전달되거나 변수 안에 담길 수 있다

### Callback 함수

- 함수는 인수로 함수를 전달받을 수 있고, 전달해 줄 수 있다
- callback function이란 이러한 매개변수로서 전달된 함수를 의미하고
- callback function은 그 함수를 전달받은 함수 안에서 호출되게 된다

### Non-Blocking Code

- 모든 비동기식 함수에서는 첫번째 매개변수로 error를, 마지막 매개변수로는 callback 함수를 받습니다
- fs.readFile() 함수는 비동기식으로 파일을 읽는 함수이고, 도중에 에러가 발생하면 err 객체에 에러 내용을 담고 그렇지 않을 시에는 파일 내용을 다 읽고 출력합니다

```
var fs = require("fs");

fs.readFile('input.txt', function (err, data) {
    if (err) return console.error(err);
    console.log(data.toString());
});

console.log("Program has ended");
```

- readFile() 메소드가 실행 된 후, 프로그램이

### Promise

- `promise` 객체는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타냅니다.

### Node에서 Redis 사용하기