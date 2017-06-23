# Webpack

webpack이란 Module Bundler 이다. webpack과 같은 모듈 번들러로는 `grunt`, `gulp` 등이 있다.

>  각각 모듈들에 필요한 의존성에 대해 관계를 파악하여 그룹핑해주며, 여러 기능 자동 수행.

![webpack에 대한 이미지 검색결과](https://webpack.github.io/assets/what-is-webpack.png)

여러가지 js, css, png 파일들이 모듈 번들러를 통해서 몇몇 파일들로 합쳐진다. 

#### 진행되야 하는 작업들

- ECMA6 파일을 ECMA5 파일로 변경
- 문서 최적화
- 코드 테스트
- 문서 문법 검증 
- SASS 컴파일
- ...

위와 같은 빌드 단계에서 진행되야 하는 것들을 일관화하여, 모듈화시켜주는데 기여한다. 

모듈화 시키는데 대표적인 작업 그룹이 있었으니 `CommonJs`와 `AMD(Asynchronous Module Definition)`가 있다.  `webpack`은 두 그룹의 명세를 모두 지원하는 javascript 도구이다.

### 사용법

typescript와 webpack 연동에 대해 다룬다.

> ```
> [폴더명]/
> ├─ public/
> └─ src/
>    └─ components/
> ```

위와 같이 폴더 구조를 만든다. src는 타입스크립트 소스코들이 들어가고, webpack을 통한 결과물은 public에 위치한다. 컴포넌트들은 src/components에 위치할 것이다.

#### Typescript

1. NPM 패키지로 전환

> \> npm init

2. Typescript 설치

>  \> npm install --save-dev typescript

`-g`를 통해 글로벌로 설치하기도 하지만, 협업환경에서는 로컬로 설치한뒤 npm 스크립트를 이용하는 경우가 일반적임.

3. Typescript 파일 작성

- src/kanban/Kanban.ts

````typescript
export class Kanban {
    show() {
        alert("Hello, Stranger?");
    }
}
````

- src/index.ts

````typescript
import { Kanban } from './kanban/Kanban';

window.onload = function () {
    var o = new Kanban();
    o.show();
}
````

- src/index.html

````html
<!DOCTYPE html>
<html>
    <head><title>TypeScript Greeter</title></head>
    <body>
        <script src="index.js"></script>
    </body>
</html>
````



4. TypeScript 설정파일

root 폴더에 `tsconfig.json`을 생성한다

````json
{
    "compilerOptions": {
        "outDir": "./public/",
        "sourceMap": true,
        "noImplicitAny": true,
        "module": "commonjs",
        "target": "es5"
    },
    "include": [
        "./src/**/*"
    ]
}
````

**compilerOptions**

- outDir : 컴파일 된 파일이 위치하는 장소
- sourceMap : 소스맵 사용 여부


> 소스맵이란 : 자바스크립트를 한 파일로 합치거나 사이즈를 줄이기 위해서 압축하거나 난독화해서 배포하는 방식을 많이 취하는데 이 방법은 성능에는 좋지만 사실 디버깅이 어려워지는 문제가 있다. 소스맵은 이 원본 소스와 최종소스를 매핑해서 추적할 수 있는 방법

- noImplicityAny : `any` 로 선언된 식과 정의에 대해 에러를 발생시킴.
- module : 모듈 표준으로 `commonjs`를 사용한다.
- target : es5 형태로 컴파일 함.


**files/include/exclude**

컴파일 대상인 파일 설정

`files` 키워드로 파일을 정하거나   ` "files": [  "src/index.ts"],`

`incude/exclude`로 경로를 지정할 수 있다.

>  더 자세한 옵션 값을 보려면
>
>  https://www.typescriptlang.org/docs/handbook/compiler-options.html



*컴파일 하려면....*

> \> tsc ./src/index.ts

위 명령어를 치면 ts가 js로 컴파일 되지만, index.html을 실행하면, **오류가 발생한다.** `export` 명령어는 webpack 모듈러를 사용하고 `commonjs`를 사용할 때만 사용할 수 있다.



#### Webpack

1. webpack 설치

> \> npm install --save webpack

2. Typescript 로더 설치

> \> npm install --save-dev awesome-typescript-loader source-map-loader

`awesome-typescript-loader`는 웹팩이 ts 표준 설정파일인 tsconfig.json을 활용해서 컴파일하게 도와주는 로더이다. `Babel`을 통해 ES5로 변환해주며, 캐시를 활용하여 webpack 컴파일을 더 빠르게 해준다.

`source-map-loader`는 웹팩이 여러 라이브러리를 관리하는 과정에서 소스맵 데이터의 연속성을 유지하기 위해 필요하다. 소스맵이 변경될때마다, 웹팩에게 알려준다.

3. 웹팩 설정

- webpack.config.js

````typescript
const path = require('path');

const config = {
  entry: './src/index.ts',
  output: {
    path: path.resolve(__dirname, 'public'),
    filename: 'index.js'
  },
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'awesome-typescript-loader' },
      { enforce: "pre", test: /\.js$/, loader: "source-map-loader" },
      { enforce: 'pre', test: /\.ts$/, loader: 'tslint-loader' }
    ]
  },
  resolve: {
    extensions: [".ts", ".js", ".json"]
  }
};

module.exports = config;
````

**Entry(진입점)** 

페이지 시작의 첫번째 파일로, Entry는 의존성 관계 파악 및 그룹핑을 위한 위치 정보를 알려주는 기능을 한다.

**Output**

웹팩의 `output` 프로퍼티는 웹팩에게 그룹핑된 코드를 어디 위치 시킬지 알려준다.

`output.filename`과 `output.path`프로퍼티는 웹팩에게 묶음의 이름과, 결과물 생성 위치를 알려준다.

**Loaders**

웹팩의 로더는 다른 리소스를 순수 JavaScript로 변환하고, css, html, scss, jpg 등을 종속성 정보에 추가함으로서 모듈로서 관리한다.

Loader의 설정은 `rules` 프로퍼티를 통해 이루어지는데, rules 프로퍼티에는 두가지 속성이 있다.

`test` : `require() / import` 문에서 '.txt'파일로 해석되는 경로를 발견하면,

`use` or `loader` : 로더의 종류를 입력.

*로더 사용하기 위해서는*

> \> npm install --save-dev css-loader awesome-typescript-loader 
>
> \> npm install --save-dev tslint tslint-loader

*참고 : tslint 기본 설정 파일 생성*

> \> ./node_modules/.bin/tslint --init 


**Resolve**
extensions를 등록해두면, import시에 확장자 생략이 가능하다.

**Plugins**

플러그인을 통해 `컴파일`이나, `chunks` 과정에서 사용자 정의 기능을  수행하는데 사용된다.  단순히 `require()` 키워드로 불러와서, `plugins` 속성에 추가하면 된다.

`html-webpack-plugin` : 컴파일 output 위치로 html 파일을 복사해주는 플러그인.

> \> npm install --save-dev html-webpack-plugin

- webpack.config.js

````javascript
...
const HtmlWebpackPlugin = require('html-webpack-plugin');
...
  plugins: [
    new HtmlWebpackPlugin({
      template: 'src/index.html'
    })
  ]
...
````

4. webpack 실행

> \> webpack

위 커맨드를 입력하면 `public/index.html` 파일이 생성된 것을 볼 수있다.

5. webpack-dev-server 사용하기

파일을 수정할 때 마다 webpack을 실행하는 것은 비효율적이다. 한 번 실행해놓고 파일을 수정하면 자동으로 반영되게 해주는 것이 `webpack-dev-server` 이다.

> \> npm install --save-dev webpack-dev-server
>
> \> ./node_modules/.bin/webpack-dev-server  --content-base public/ --open

`--content-base` : 소스파일 베이스 설정

`--open` : 브라우저를 실행

실행 경로를 쓰기 귀찮으니, `package.json`에 다음을 추가

````json
...
  "scripts": {
    ...
    "build": "webpack",
    "start": "webpack-dev-server --content-base public/ --open"
  },
````

> \> npm start

입력하면 경로 없이 dev-server 실행!




## Reference

웹팩&타입스크립트 연동 : https://www.typescriptlang.org/docs/handbook/react-&-webpack.html

https://www.facebook.com/218158748272233/videos/993595284061905/

https://hyunseob.github.io/2017/03/21/webpack2-beginners-guide/

tslint-loader : https://www.npmjs.com/package/tslint-loader

awesome-typescript-loader : https://github.com/s-panferov/awesome-typescript-loader

source-map-loader : https://github.com/webpack-contrib/source-map-loader

소스맵이란 : https://blog.outsider.ne.kr/916

html-webpack-plugin, html-webpack-plugin : http://sixmen.com/ko/tech/2017-04-11-1-webpack-setup-tutorial-with-typescript-and-mithril/

