# Angular 2 프로젝트의 구성

앵귤러2의 프로젝트는 어떻게 구성되어 있는지 정리해보자

- ./app
- ./index.html
- ./node_modules
- ./package.json
- ./styles.css
- ./systemjs.config.js
- ./tsconfig.json
- ./typings.json



## package.json

npm 명령을 통해 외부 모듈에 대한 "의존성 관리"를 해준다. 

````json
{
  "name": "angular-quickstart",
  "version": "1.0.0",
  "scripts": {
    "start": "tsc && concurrently \"tsc -w\" \"lite-server\" ",
    "lite": "lite-server",
    "postinstall": "typings install",
    "tsc": "tsc",
    "tsc:w": "tsc -w",
    "typings": "typings"
  },
  "licenses": [
    {
      "type": "MIT",
      "url": "https://github.com/angular/angular.io/blob/master/LICENSE"
    }
  ],
  "dependencies": {
    "@angular/common": "~2.0.2",
    "@angular/compiler": "~2.0.2",
    "@angular/core": "~2.0.2",
    "@angular/forms": "~2.0.2",
    "@angular/http": "~2.0.2",
    "@angular/platform-browser": "~2.0.2",
    "@angular/platform-browser-dynamic": "~2.0.2",
    "@angular/router": "~3.0.2",
    "@angular/upgrade": "~2.0.2",
    "angular-in-memory-web-api": "~0.1.5",
    "bootstrap": "^3.3.7",
    "core-js": "^2.4.1",
    "reflect-metadata": "^0.1.8",
    "rxjs": "5.0.0-beta.12",
    "systemjs": "0.19.39",
    "zone.js": "^0.6.25"
  },
  "devDependencies": {
    "concurrently": "^3.0.0",
    "lite-server": "^2.2.2",
    "typescript": "^2.0.3",
    "typings":"^1.4.0"
  }
}
````

기본 설정 값으로 package.json파일을 만드려면

> $ npm init --yes

설정한 pacakge.json 파일대로 패키지 설치하려면

> $ npm install

개발 서버 실행

> $ npm start

- 자주 사용하는 npm 명령어

| 명령어                          | 의미                       |
| ---------------------------- | ------------------------ |
| npm install --global 패키지명    | 패키지를 로컬 전역에 설치함          |
| npm install --global 패키지명@버전 | 패키지의 특정 버전을 로컬 전역에 설치함   |
| npm install -g 패키지명@latest   | 패키지의 최신 버전을 로컬에 설치함      |
| npm uninstall -g 패키지명        | 로컬에 설치된 패키지를 삭제          |
| npm view 패키지명 versions       | 원격 저장소에 등록된 모든 패키지 버전 확인 |
| npm view 패지지명 version        | 원격저장소에 등록된 최신 패키지 버전만 확인 |

## typings.json

타입스크립트가 외부라이브러리(node, jasmine, core-js)를 인식할 수 있게 한다.

## tsconfig.json

타입스크립트를 자바스크립트로 변환할 때 필요한 설정 정보.

## index.html

프로젝트의 시작 페이지.

````html
<!DOCTYPE html>
<html>
  <head>
    <title>Hello Angular 2 Application</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 첫번째. 전역 스타일 호출 -->
    <link rel="stylesheet" href="styles.css">

    <!-- 두번째. 폴리필 라이브러리 호출 -->
    <script src="https://unpkg.com/core-js/client/shim.min.js"></script>
    <script src="https://unpkg.com/zone.js@0.6.25?main=browser"></script>
    <script src="https://unpkg.com/reflect-metadata@0.1.3"></script>

    <!-- 세번째. SystemJS 라이브러리 호출과 설정 -->	
	<script src="https://unpkg.com/systemjs@0.19.27/dist/system.src.js"></script>
    <script src="systemjs.config.js"></script>
    <script>
      System.import('app').catch(function(err){ console.error(err); });
    </script>
  </head>  
  <body>
	<!-- 네번째. application 표시 -->
    <my-app>Loading...</my-app>
  </body>
</html>
````

- **styles.css**

  앱 전역 스타일링

- **폴리필 라이브러리 임포트**

  > 폴리필? 구버전의 브라우저가 최신 스크립트 기능에 대응하도록 보완하는 라이브러리

  - core-js : ES6 지원을 위한 폴리필
  - zonde.js : 변화 감지를 위한 라이브러리
  - reflect-metadata : ES7 장식자 추가를 위한 폴리필

- **SystemJS 라이브러리**

  동적 모듈 로더, systemjs.config.js 파일을 통해 모듈 구성

  ````javascript
  (function (global) {
    System.config({
      paths: {
        // 경로에 대한 별칭 설정
        'npm:': 'node_modules/'
      },
      map: {
        app: 'app',
        '@angular/core': 'npm:@angular/core/bundles/core.umd.js',
        '@angular/common': 'npm:@angular/common/bundles/common.umd.js',
        '@angular/compiler': 'npm:@angular/compiler/bundles/compiler.umd.js',
        '@angular/platform-browser': 'npm:@angular/platform-browser/bundles/platform-browser.umd.js',
        '@angular/platform-browser-dynamic': 'npm:@angular/platform-browser-dynamic/bundles/platform-browser-dynamic.umd.js',
        '@angular/http': 'npm:@angular/http/bundles/http.umd.js',
        '@angular/router': 'npm:@angular/router/bundles/router.umd.js',
        '@angular/forms': 'npm:@angular/forms/bundles/forms.umd.js',
        // 다른 라이브러리
        'rxjs':                      'npm:rxjs',
        'angular-in-memory-web-api': 'npm:angular-in-memory-web-api',
      },
      // 파일이름이 없거나 확장자가 없을때의 처리방법에 대한 설정
      packages: {
        app: {
          main: './main.js',
          defaultExtension: 'js'
        },
        rxjs: {
          defaultExtension: 'js'
        },
        'angular-in-memory-web-api': {
          main: './index.js',
          defaultExtension: 'js'
        }
      }
    });
  })(this);
  ````

  - paths 필드 : 경로에 대한 별칭
  - map 필드 : 모듈 호출 정의
     - app 필드 : 어플리케이션 디렉터리 위치 설정
     - node_module의 별칭 (npm:@angular/..)으로 Angular 모듈 임포트
     - Angular모듈은 @을 기재해서 외부 라이브러리와 구분
  - packages : 확장자 처리

- <my-app>

  Angular 앱이 실행되는 위치이다. my-app은 어플리케이션 최상위 컴포넌트이다.

  - app.component.ts

  ````javascript
  import { Component } from '@angular/core';

  @Component({
    selector: 'my-app',
    template: '<h1>Hello Angular 2 Application</h1>'
  })
  export class AppComponent { }
  ````

  selector의 이름이 <my-app>에 대응하는 것을 확인할 수 있다.

  - app.module.ts

  ````typescript
  import { NgModule }      from '@angular/core';
  import { BrowserModule } from '@angular/platform-browser';

  import { AppComponent }   from './app.component';

  @NgModule({
    imports:      [ BrowserModule ],
    declarations: [ AppComponent ],
    bootstrap:    [ AppComponent ]
  })

  export class AppModule { }
  ````

  애플리케이션 차원의 모듈파일. 전역으로 사용되는 모듈이나 최상위 컴포넌트를 등록. bootstrap 속성에는 애플리케이션을 실행할 때 가장 먼저 호출하게 할 컴포넌트를 등록

  - main.ts

   ````typescript
  import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

  import { AppModule } from './app.module';

  const platform = platformBrowserDynamic();
  platform.bootstrapModule(AppModule);
   ````

  app.modules.ts 파일을 임포트 해서 platform-browser-dynamic을 등록.

  > platform-browser-dynamic은 Angular의 모든 API에 대한 진입점.

  platform의 bootstrapModule() 메서드에 방금 정의한 AppModule을 지정.



## Reference

쉽고 빠르게 배우는 Angular 2 프로그래밍 - 정진욱

