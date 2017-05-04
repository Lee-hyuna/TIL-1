## Angular 2.0 구조

Jquery를 써서 웹페이지를 작성해도

크게 문제될 것 없다.



그러나, 비동기 페이지를 작성하다보면

상당한 하드코딩이 필요하다.



하지만 Angular를 사용한다면,

비동기 페이지를 작성하는데 뛰어난 생산성을 가져다준다.



### 특징

- **데이터 바인딩**

Jquery는 직접 DOM을 건드려 데이터를 수정한다면.

Angular에서는 데이터를 바인딩시켜,

데이터가 변경될 때마다 자동으로 내용을 동기화시켜준다.



- **html 과 프론트 엔드 코드의 분리**


````javascript
@Component({
	templateUrl: 'app.component.html',
    // ...
})
````



- **TypeScript 사용**




### 컴포넌트

Angular 하나의 어플리케이션은 컴포넌트들의 조합으로 이루어진다.

Component는 Template + Class + Metadata로 이루어진다.



- Template

Html로 작성된다.

바인딩(Binding), 지시자(Directive)를 포함한다.



*지시자는 자신만의 HTML태그를 만들어*

*상태와 액션을 담아 컴포넌트화 시킬 수 있는 앵귤러만의 기능이다.*



- Class

Properties와 Method를 포함한다.

TypeScript형태로 작성된다.



- Metadata

Anuglar에 대한 추가에디터로서, Decorator로 정의한다.



#### 컴포넌트 생성

````typescript
@Component({
  selector: 'hello-world', 
  template: '<div>template</div>', 
  styles: ['div { background: blue; }'] })
export class HelloWorld { }
````

이렇게 컴포넌트를 등록하면 html파일이나, 다른 컴포넌트에

> <hellow-world>

형식으로 사용할 수 있다.



#### 컴포넌트 모듈 등록

일반적으로 app.module.ts에 등록함.

````typescript
import { MyComponent } from './my.component';
@NgModule({
  declarations : [
    MyComponent
  ],
  ...
})
````



#### 컴포넌트 계층 구조

- child

````typescript
import { Component } from '@angular/core';
@Component({ 
  selector: 'child', 
  template: '<div>자식</div>', 
  styles: ['div { border: 2px solid black; margin: 10px; padding: 10px; }] 
}) 
export class ChildComponent { }
````

- parent

````typescript
import { Component } from '@angular/core';
@Component({ 
  selector: 'parent', 
  template: ` <div> 부모 <child></child> </div>`, 
  styles: ['div { border: 2px solid red; margin: 10px; padding: 10px; }'] 
})
export class ParentComponent { }
````





#### 데이터 바인딩 방법

- 부모 -> 자식

`[]` 기호를 통해 자식 컴포넌트에게 데이터 바인딩 가능.

````typescript
// 부모
@Component({ 
  selector: 'parent', 
  template: ` 
  <div> 부모
	<child [data1]="data"     // 필드 데이타
		   [data2]="data2()" // 함수
		   [data3]="data3"   // 배열
		   [data4]="1+1" // 연산
            [data5]="data5" // static
            [data6]="data6"> // getter (in typescript) 
	</child>
  </div> `
})
export class ParentComponent { 
  data = 1; 
  data2() { 
    return "data2";
  } 
  data3 = ['one', 'two', 'three']; 

  static data5 = "Five";
  get data6() { 
    return ParentComponent.data5; 
  } 
}
````

````typescript
// 자식 방법 1
import { Component, Input } from '@angular/core';

@Component({ 
  selector: 'child', 
  template: `
	<div>
		<h2> 자식 </h2> 
		{{data1}}, {{data2}}, {{data3}}, {{data4}}, {{data5}}, {{data6}}
	</div>` 
})
export class ChildComponent { 
  @Input() data1: number; 
  @Input() data2: string; 
  @Input() data3: string[];
  @Input() data4: number; 
  @Input() data5: string; 
  @Input() data6: string;
}
````

````typescript
// 자식 방법 2 input 속성 이용
import { Component } from '@angular/core';
@Component({ 
  selector: 'child', 
    template: `
	<div>
		<h2> 자식 </h2> 
		{{data1}}, {{data2}}, {{data3}}, {{data4}}, {{data5}}, {{data6}}
	</div>`, 
  	inputs: ['data1', 'data2', 'data3', 'data4', 'data5', 'data6']
})
export class ChildComponent { 
  data1: number; 
  data2: string; 
  data3: string[]; 
  data4: number;
  data5: string; 
  data6: string; 
}
````

- 자식 -> 부모도 가능.



## Reference

[비주얼라이즈 ](http://visualize.tistory.com/442)

[React보다 Angular2에 더 주목해야하는 이유 | 손찬욱 Blog](http://sculove.github.io/blog/2016/07/11/react%EB%B3%B4%EB%8B%A4-angular2%EC%97%90-%EB%8D%94-%EC%A3%BC%EB%AA%A9%ED%95%B4%EC%95%BC%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0/)



출처: http://sticky32.tistory.com/entry/Angular2-컴포넌트에-대해서 [Sticky]







