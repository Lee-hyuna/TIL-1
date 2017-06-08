# SCSS

## 변수

````scss
$변수명: #ff0000;
div{
  color: $변수명;
}
````



## 스타일 변수 처리

> 선언 : @mixin mixin명 {}
>
> 호출 : @include mixin명 {}

````scss
@mixin boxStyle {
  border : 1px $red #000;
}

.box {
  @include boxStyle;
}
````



> 선언 : @mixin mixin명(파라미터){}
>
> 호출 : @include mixin명(파라미터)

````scss
@mixin style($color, $size) {
  border : $size $color #000;
}

.box {
  @include style(#fff064, 10px)
}
````



## 상속

````scss
.menu {
  width: 200px;
}
.menu1 {
  @extend .menu;
}
````



## 부모참조선택자

- scss

````scss
$red: #ff0000;
div {
  color: $red;
  & span {
    color: $red;
  }
}
````

- css

````typescript
div { color: #ff0000; }
div span { color: #ff0000; }
````



## 컴파일

scss가 존재하는 경로로 이동해서

> \> sass --style **nested** 작성된scss명.scss 바꿔줄css명.css      <- 기본
>
> \> sass --style **expanded** 작성된scss명.scss 바꿔줄css명.css      <- 확장
>
> \> sass --style **compact** 작성된scss명.scss 바꿔줄css명.css      <- 축약



#### 자동 컴파일

> \> sass --watch 작성된.scss
>
> \> sass --watch --style 스타일종류 작성된.scss
>
> \> sass --watch .:. 
>
> \> sass --watch --style 스타일종류 .:. 



## Refenrece

 [회복맨 블로그] : http://recoveryman.tistory.com/277