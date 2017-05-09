# Node.js란?

Node.js를 이해하기 위해서는 먼저 javascript에 대해 이해할 필요가 있음.

##자바스크립트

자바스크립트는 넷스케이프 웹 브라우저에서 HTML의 DOM(Document Object Model) 객체를 제어하기 위해 개발되었음.

초창기에는 매우 느렸으나, 구글이 크롬과 함께 V8 자바스크립트 엔진을 개발하면서, 기존 웹브라우저에 포함된 엔진보다 월등히 빨라진 성능을 보였음.

기존 JS엔진들은 자바스크립트를 바이트코드로 변환하거나 인터프리트하여 처리했지만 V8은 JIT(Just In Time) 컴파일 방식을 사용하여 성능을 획기적으로 개선.

> JIT 컴파일 방식은 자바스크립트를 인터프리트하지 않고 실행 즉시 기계어(x86, ARM등)로 컴파일

## Node.js

2009년 라이언 달이라는 프로그래머가 구글의` V8 자바스크립트 엔진`을 웹 브라우저가 아닌 서버로 사용할 수 있도록 만든 것이 Node.js

- JS의 간결함
- V8 JS 엔진의 월등한 속도
- 단일 스레드(Non-bloking I/O)
- 소켓 통신
- HTTP 웹서버로 실행 가능
- npm(Node Packaged Modules)은 Node.js로 만들어진 모듈을 인터넷에서 받아서 설치해주는 패키지 매니저.

## 단일 스레드 모델, Non-blocking I/O

멀티 스레드 방식은 복잡한 동기화 문제가 골칫거리 였음. 동기화 모델이나 락에 대해 학습해야 하고 쓰기가 상당히 어려워 생산성 저하로 이어짐.

Node.js는 멀티스레드 모델 대신 단일 스레드 모델과 Non-blocking I/O를 채택.

- **Blocking 방식**

````c
#include <stdio.h>

int sum(int a, int b)
{
    return a + b;
}

int main()
{
   int result;
   result = sum(1, 2);          // 1
   printf("sum: %d", result);   // 2

   return 0;                    // 3      
}
````

위 C언어 코드의 실행은 그냥 줄 순서, printf 함수로 결과를 찍으려면 sum 함수가 끝날 때까지 기다려야 함. 이를 `Blocking` 방식이라고 함.

- **Non-Blocking 방식**

````javascript
var fs = require('fs');

fs.readFile('./컴배콤.txt' /* 1 */, function (err, data) { 
  console.log(data); // 3
});

console.log('Hello JavaScript'); // 2
````

`fs.readFile()`의 결과값이 나올때까지 기다리지 않고, 그다음 줄 `console.log('Hello JavaScript');`를 실행해버림. 시간 경과 후 파일을 다 읽었으면 `console.log(data)`가 실행됌.

> 총 2개의 스레드로 작동 : 시간이 오래 걸리는 작업은 워커 스레드로 보내고, 메인 스레드는 코드를 계속 실행. 워커 스레드에서 작업이 끝나면 결과를 다시 메인 스레드로 보냄.



## Reference

http://pyrasis.com/nodejs/nodejs-HOWTO