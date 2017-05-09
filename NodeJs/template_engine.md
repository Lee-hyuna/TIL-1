# Node JS 템플릿엔진

express로 웹서버를 만들더라도 파일을 하나하나 정의하기에는 무리가 있음. express에서 템플릿 엔진을 사용하면 PHP나 JSP처럼 서버에서 HTML을 동적으로 생성할 수 있음.

## EJS(Embedded JavaScript)

서버에서 자바스크립트로 HTML을 생성하는 템플릿 엔진

- 설치 및 실행

> $ npm install -g express-generator
>
> $ express --ejs

- 구조
  - app.js
  - bin
    - www
  - package.json
  - public
    - images
    - javascripts
    - stylesheets
      - style.css
  - routes
    - index.js
    - users.js
  - views
    - error.ejs
    - index.ejs

> $ npm install

생성후 위 명령어만 입력하면 package.js에 정의된 모듈이 모두 설치됌. bin 디렉터리 아래 `www` 파일이 생성되는데 node로 실행

> $ node bin/www

웹 브라우정에서 **http://127.0.0.1:3000** 으로 접속하면. 빈화면에 **Express Welcome to Express** 라고 표시됌.

- EJS 기본 문법

`view/index.eja`

````html
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
  </body>
</html>
````

`<% %>` 안에 자바스크립트를 사용하면 됌. 변수 출력은 `<%= 변수 %>`로 가능.

- Welcome to Express 5번 출력하기

```html
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <% for (var i = 0; i < 5; i++) { %>
    <p>Welcome to <%= title %></p>
    <% } %>
  </body>
</html>
```

- title 변수의 위치

`routes/index.js`

```javascript
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res) {
  res.render('index', { title: 'Express' });
});

module.exports = router;
```

템플릿 엔진을 이용해서 HTML 생성 가능 아래 사이트 참조

[https://code.google.com/p/embeddedjavascript/wiki/ViewHelpers](https://code.google.com/p/embeddedjavascript/wiki/ViewHelpers)

## Jade

HTML을 간략화한 템플릿 엔진

> $ npm install -g express-generator
>
> $ express

express-generator에 아무 옵션을 주지 않으면 Jade로 어플리케이션을 생성

- 구조
  - app.js
  - bin
    - www
  - package.json
  - public
    - images
    - javascripts
    - stylesheets
      - style.css
  - routes
    - index.js
    - users.js
  - views
    - error.ejs
    - index.ejs

EJS와 같이 `npm install` 입력하면 package.js에 정의된 모듈들이 설치.

> $ node bin/www

http://127.0.0.1:3000으로 접속하면 **Express Welcome to Express**라고 나오는 것을 확인.

- views/layout.jade

```jade
doctype html
html
  head
    title= title
    link(rel='stylesheet', href='/stylesheets/style.css')
  body
    block content
```

- views/index.jade

```jade
extends layout

block content
  h1= title
  p Welcome to #{title}
```

`index.jade`는 `extends layout` 문법을 이용하여 `layout.jade`파일을 상속.

layout.jade, index.jade 파일 모두 `block content`가 있음.

layout.jade 파일에서 block을 선언하고, index.jade 파일에서 block을 정의

- 변수 출력 :  ex) `h1= title` 
- 변수 할당 : `${ }`


- jade의 예제

````jade
extends layout

block content
  h1= title
  p Welcome to #{title}

  #{ items = ["one", "two", "three"] }
  each item, i in items
    li #{item}: #{i}
````

`routes/index.js`

````jade
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res) {
  res.render('index', { title: 'Express' });
});

module.exports = router;
````



## Reference

http://pyrasis.com/nodejs/nodejs-HOWTO