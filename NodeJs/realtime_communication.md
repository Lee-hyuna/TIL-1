# Node JS 실시간 통신

소켓 통신을 하기 위한 Node.JS에 대표적인 통신기술들

- TCP Socket
- Web Socket
- socket.io


https://github.com/skout90/TIL/blob/master/Web/socket.md

## WebSocket

- 모듈 설치

> $ npm install websocket

- app.js 생성

````javascript
var WebSocketServer = require('websocket').server;
var http = require('http');

var server = http.createServer(function (req, res) {
  console.log('Received request for ' + req.url);
  res.writeHead(404);
  res.end();
});

// 클라이언트 접속을 대기
server.listen(8000, function () { 
  console.log('Server is listening on port 8000');
});

// WebSocket객체의 wsSever생성
wsServer = new WebSocketServer({
  httpServer: server,
  autoAcceptConnections: false
});

// 서버에 request 요청이 오면
wsServer.on('request', function (request) {
  // 연결 객체를 만듬, 이름을 지정 'echo'
  var connection = request.accept('echo', request.origin); 
  // 메세지가 오면 처리할 함수 지정
  connection.on('message', function (message) {
    if (message.type === 'utf8') {
      console.log('Received message: ' + message.utf8Data);
      // 클라이언트에게 메세지를 보냄.
      connection.sendUTF(message.utf8Data);
    }
    else if (message.type === 'binary') {
      connection.sendBytes(message.binaryData);
    }

    connection.on('close', function (reasonCode, description) {
      console.log('Peer ' + connection.remoteAddress + ' disconnected.');
    });
  });
});
````

- index.html

````html
<!DOCTYPE HTML>
<html>
<head>
  <title>Example WebSocket</title>
</head>
<body>

<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
<script type="text/javascript">
// 웹소켓이 브라우저에 존재하면
if ('WebSocket' in window) {
  // 웹소켓 서버주소와 프로토콜(echo)을 지정하여, WebSocket 객체를 만듦.
  var ws = new WebSocket('ws://127.0.0.1:8000', 'echo');

  // 웹소켓이 연결되었을때
  ws.onopen = function () {
    $('#status').text('connected');

    for (var i = 0; i < 10; i++) {
      ws.send('Hello ' + i);
    }
  };

  // 메세지를 받았을때
  ws.onmessage = function (evt) {
    $('#messages').append($('<li>').text('Received message: ' + evt.data));
  };

  // 연결이 끊어졌을때
  ws.onclose = function () {
    $('#status').text('connection is closed');
  };
}
else
  $('#status').text('WebSocket not supported.');
</script>

Status: <span id="status"></span><br /><br />
Messages: <ul id="messages"></ul>
</body>
</html>
````

- 실행

> $ node app.js

`app.js` 실행후, `index.html` 파일을  더블클릭하여 웹브라우저에서 열면 서버로부터 받은 메세지가 나오는 것을 확인할 수 있다.

## socket.io

서버와 클라이언트가 한번씩 메세지를 주고 받는 예제 작성

- 모듈 설치

> $ npm install socket.io

- **app.js**

````javascript
// 클라이언트의 접속을 대기
var io = require('socket.io').listen(8000);

// 클라이언트가 접속할 시에 실행
io.sockets.on('connection', function (socket) {
  // 'server' 이름으로 메시지 전송
  socket.emit('server', { hello: 'world 1' });

  // 'client' 이름의 메시지 수신했을 때
  socket.on('client', function (data) {
    console.log(data);
  });
});
````

- **index.html**

````html
<!DOCTYPE HTML>
<html>
<head>
  <title>Example Socket.IO</title>
</head>
<body>

<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
<script src="http://127.0.0.1:8000/socket.io/socket.io.js"></script>
<script type="text/javascript">
var socket = io.connect('http://127.0.0.1:8000');
// 'server'라는 이름의 메세지가 왔을때 
socket.on('server', function (data) {
  $('#messages').append($('<li>').text('Received message: ' + data.hello));
});
// 'client'라는 이름으로 메세지 송신
socket.emit('client', { hello: 'world 2' });
</script>
Messages: <ul id="messages"></ul>
</body>
</html>
````

- 실행

> $ node app.js

`app.js` 실행후, `index.html` 파일을  더블클릭하여 웹브라우저에서 열면 서버로부터 받은 메세지가 나오는 것을 확인할 수 있다.



## Reference

http://pyrasis.com/nodejs/nodejs-HOWTO

