# Node JS 웹서버 만들기

## NodeJS 다운로드

https://nodejs.org/ko/

## 사용하기

- **app.js 파일 생성**

````javascript
console.log('Hello World');
````

> $ node app.js

터미널에서 위 명령어 입력하여 app.js 실행.

## 웹 서버 만들기

- **app.js**

````javascript
var http = require('http');

var server = http.createServer(function (req, res) {
  res.writeHead(200, { 'Content-Type' : 'text/plain' });
  res.end('Hello World');
});

server.listen(8000);
````

> $ node app.js

명령어 입력 하면, `server.listen()`함수로 인해 8000포트로 접속을 대기함. 클라이언트가  http://127.0.0.1:8000 으로 접속하면 Hello World가 나옴.

## express로 웹 서버 만들기

`express`는 Node.js에서 가장 유명한 웹 프레임워크 모듈. 더 간단하고 다양한 템플릿 엔진 기능을 활용해서 웹서버를 만들 수 있음.

- **app.js 작성**

````javascript
var express = require('express') // 모듈 로딩
  , http = require('http')
  , app = express()
  , server = http.createServer(app); // app객체와 http 서버 연결

app.get('/', function (req, res) { // HTTP GET 메서드 처리
  res.send('Hello /');
});

app.get('/world.html', function (req, res) {
  res.send('Hello World');
});

server.listen(8000, function() { // 8000 번 포트로 서버 실행
  console.log('Express server listening on port ' + server.address().port);
});
````

- **express를 설치**

> $ npm install express

app.js가 있는 디렉터리에서 명령어 실행할것, 명령을 실행하면 디렉터리에 node_modules 디렉터리가 생성됌. 그 안에 express 모듈들이 존재.

전역 설치를 위해서는

> $ npm install -g express

그러면 app.js 디렉터리와 상관없이 모듈 사용 가능.

- **실행**

> $ node app.js

웹 브라우저에서 **http://127.0.0.1:8000** 으로 접속하면 빈화면에 **Hello /**가 표시되고 **http://127.0.0.1:8000/world.html**로 접속하면 **Hello World**가 표시



## Reference

http://pyrasis.com/nodejs/nodejs-HOWTO