## Angular 2.0 구조

Jquery를 써서 웹페이지를 작성해도

크게 문제될 것이 없었다.



그러나, 비동기 페이지를 작성하다보면

하드코딩이 필요한 불편한 점이 있었다.

그렇게 접한 Angular는 답답했던 속을 뻥 뚫어주었다.

비동기 페이지를 작성하는데 뛰어난 생산성을 가져다주었기 때문이다.



충분히 만족하며 쓰고 있었지만....

2015년 4월 Angular2가 발표되었고.

단순한 확장이 아니라, 아예 새로운 프레임워크 였기에.

Angular2가 발표된 직후, 개발자들이 "죽음", "재앙"과 같은 표현을 썼다고

(나만 그렇게 생각했던게 아니었구나..)



그런데, 생각보다 큰 장점이 많다고 한다.

Angular2에서 바뀐 부분을 소개 한다.



#### Angular2에서 바뀐 것들

- **컴포넌트 기반 개발**

Controller 메서드가 아닌 컴포넌트 중심으로 프로그래밍

- **영역 구분**

$scope를 사용하지 않아도, 컴포넌트에 의해 영역이 명확히 구분

- **Dom제어 모듈**

angular.module 없이도 향상된 모듈 시스템 제공

jQlite의 기능을 대체하는 Dom 제어 모듈 제공

- **API 단순화**

AngularJS의 지시자 43개를 폐기함으로서 API가 단순해짐

- **성능향상**

Angular1은 digest 루프로 인한 성능 저하가 있었음.

Angular2에서는 이 문제가 발생하지 않음.

그 외에도 성능 저하요인들이 사라지고 나서

로딩시간은 2.5배, 바인딩을 통한 렌더링 성능은 4.2배 빨라짐

- **TypeScript 사용**


 자바스크립트의 상위집합 언어.

따라서 자바스크립트 문법을 그대로 이용할 수 있음.

최신 ECMA 스크립트 표준인 EC6, EC7의 특징까지도 지원

- **컴파일 방식 변경**

AOT(Ahead of time compilation)과 같은 사전 컴파일 방식 도입.

HTML 템플릿과 CSS파일을 컴파일해 코드로 삽입하는 방식.

> ex) ngIf나 ngFor와 같은 지시자를 브라우저에서 직접 실행할 일 없으니,
>
> 컴파일 과정에 미리 수행해 코드에 적재해 놓고, 컴파일 없이 곧바로 실행되게 함.

컴파일 과정이 없으므로 화면 표시 속도가 빠름.

코드 용량도 50%이상까지 최적화.(JIT 컴파일러를 적재하지 않아도 되기 때문)



### Angular2의 구조

Angular 하나의 어플리케이션은 컴포넌트들의 조합으로 이루어진다.

**Component는 Template + Class**로 이루어진다.

컴포넌트들 끼리는 Routing이 가능하며.

모듈과 서비스를 두어 중복 코드를 최소화 시킨다.



- **Template**

템플릿은 **컴포넌트의 UI**를 나타냄.

Html로 작성.

컴포넌트마다 가상 DOM을 이용하므로 컴포넌트간 스타일에 영향을 받지 않음.



템플릿은

바인딩(Binding), 지시자(Directive)를 포함한다.

템플릿은 바인딩을 통해 클래스와 이어지는데..



- **Class**

바인딩을 통해 템플릿과 연결됌.

> 이를 통해 템플릿에서 클래스로 이벤트를 전달하거나, 데이터를 교환할 수 있음.

Properties와 Method를 포함

TypeScript형태로 작성.



- **Routing**

컴포넌트는 자식 컴포넌트를 포함시킬 수 있음

다른 컴포넌트로 URL을 라우팅할 수 있음(URL이 변경)



- **Module**

컴포넌트는 외부의 도움을 받아 기능을 더하거나 중복을 최소화.

이 방법으로 "모듈"을 이용.



- **Service**

컴포넌트 내부 중복로직을 최소화하기 위해 외부에 서비스를 둠.

서비스를 컴포넌트에 사용할 때 의존성 주입을 이용.



#### 컴포넌트(Component) 생성

````typescript
import { Component } from '@angular/core';
@Component({
  selector: 'hello-world', 
  template: '<div>{{msg}}</div>', 
  styles: ['div { background: blue; }'] })
export class HelloWorld {
  msg: string = "hello";
}
````

- **정의**

@Component 장식자를 이용해 정의.

- **지시자**

selector 속성에 컴포넌트를 등록하면 html파일이나, 다른 컴포넌트에

> <hellow-world>

형식으로 사용할 수 있다.

- **바인딩**

클래스에 선언된 msg 변수가 템플릿 표현식 {{msg}}에 바인딩돼 있으므로

실행시 {{msg}}위치에 hello가 들어감.



#### 모듈

- **정의**

````typescript
export class Hello {}
````

export 키워드를 이용해 모듈을 정의하고, 모듈을 외부로 노출할 것임을 알림.

- **사용**

````typescript
import { Hello } from './hello';
````

- 모듈 등록

Angular의 모듈을 체계적으로 관리하기 위해 모듈시스템을 제공.(**app.module.ts**)

@NgModule 장식자를 이용해 일반적인 모듈 구성.

````typescript
import { MyComponent } from './my.component';
@NgModule({
  imports: ]
  	Angular 모듈, routing 모듈, ...
  ],
  declarations : [
    컴포넌트, 지시자, 파이프,
  ],
  providers: [서비스 모듈, ...]
  ,
  ...
})
````



#### 서비스

컴포넌트에 제공할 목적으로 외부에 정의한 클래스.

서비스 정의는 @Injectable 장식자를 사용.

````typescript
import { Injectable } from '@angular/core';
export class HelloService {}
````



#### 지시자

- **정의**

@Directive 장식자 이용

````typescript
import { Directive, ElementRef, Render } from '@angular/core';
@Directive({
  selector: '[helloStyle]'
})
export class HelloStyleDirective {
  constructor(private el: ElementRef, private renderer: Renderer){}
}
````

- **사용**

````javascript
@Component({
  selector: 'my-component',
  template: '<div helloStyle></div>'
})
````



#### 지원

- IE9 이상
- 엣지 13 이상
- 최신버전 크롬/파이어폭스
- IE 모바일 11이상
- IOS7 이상, 안드로이드 4.1 이상




## Reference

쉽고 빠르게 배우는 Angular2 프로그래밍(장진욱 지음)

[비주얼라이즈 ](http://visualize.tistory.com/442)

[React보다 Angular2에 더 주목해야하는 이유 | 손찬욱 Blog](http://sculove.github.io/blog/2016/07/11/react%EB%B3%B4%EB%8B%A4-angular2%EC%97%90-%EB%8D%94-%EC%A3%BC%EB%AA%A9%ED%95%B4%EC%95%BC%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0/)

[http://sticky32.tistory.com/entry/Angular2-컴포넌트에-대해서](http://sticky32.tistory.com/entry/Angular2-컴포넌트에-대해서)







