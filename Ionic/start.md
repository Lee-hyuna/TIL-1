# 아이오닉 시작하기

아이오닉을 시작하기 위해서

기본 셋팅과, 명령어를 알아본다.

## Node 설치

https://nodejs.org

설치 후 커맨드 창을 열어

> $ node
>
> \> console.log('hello node');
>
> hello node
>
> undefined

와 같이 나온다면 성공.

## Cordova 설치

코도바는 하이브리드 앱에서 네이티브 앱의 기능을 사용할 수 있게 하는 라이브러리다.

> npm install -g cordova



## CLI 설치하기

CLI는 아이오닉 프레임워크 개발에 가장 기초가 되는 툴이다.

start/build/serve/run/emulate/info 같은 명령어를 사용하기 위해서는

CLI 툴을 설치해야 한다.

  

node와 npm이 설치 되어 있어야 한다.

> $ npm install -g ionic

  

## 사용하기

- 새 프로젝트 만들기

> $ ionic start myApp blank

blank 대신 다른 템플릿을 입력할 수도 있다. (blank/sidemenu/tabs)

- 정보 보기

> $ ionic info

- 디버깅

> $ ionic serve

- 업로드

> $ ionic upload



- 사용가능 명령어

> $ ionic .



#### Reference

[Ionic docs](http://ionicframework.com/docs/cli/)

