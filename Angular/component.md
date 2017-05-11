# Angular Component

Angular 컴포넌트는 W3C 표준인 웹 컴포넌트 기술을 기반으로 함. 이 웹 컴포넌트는 HTML, CS, JS를 하나의 단위로 묶어주는 기술.

## 웹 컴포넌트의 요소

- HTML 템플릿

  컴포넌트의 UI를 표현하는 영역. 재사용 가능하다는 특징.

- 템플릿 호출

  링크 엘리먼트를 이용해 호출

  `<link rel="import" href="template.html">`

- 쉐도우 DOM

  쉐도우 DOM은 문서 DOM 외부에 존재하기 때문에 문서 DOM에서 정의한 스타일이나 이벤트가 적용되지 않고 캡슐화됌.

  ````typescript
  // 쉐도우 DOM으로 <some-component> 와 같이 상세내용은 캡슐화되어 보이지않음.
  @Component({
    selector: 'some-component',
    template: `
      <h1>I am Shadow DOM!</h1>
      <h2>Nice to meet you :)</h2>
      <ng-content></ng-content>
    `;
  })
  class SomeComponent { /* ... */ }
  ````

  ````typescript
  // DOM으로 캡슐화되지 않아, 사용자가 볼 수 있음.
  @Component({
    selector: 'another-component',
    directives: [SomeComponent],
    template: `
      <some-component>
        <h1>Hi! I am Light DOM!</h1>
        <h2>So happy to see you!</h2>
      </some-component>
    `
  })
  class AnotherComponent { /* ... */ }
  ````

- 커스텀 엘리먼트

  이름을 임의로 정의해서 만든 엘리먼트 `<hello-button></hello-button>`

> 앵귤러2에서는 웹 컴포넌트를 활용하여 html의 <nav> 블록 요소들을 컴포넌트화시켜 사용합니다.



## 컴포넌트(Component) 생성

```typescript
import { Component } from '@angular/core';
@Component({
  selector: 'hello-world', 
  template: `
	<div>{{msg}}</div>
	<textarea [(ngModel)]="title"></textarea>`,
  styles: ['div { background: blue; }'] })
export class HelloWorld {
  msg: string = "hello";
}
```

- **정의**

@Component 장식자를 이용해 정의.

- **지시자**

selector 속성에 컴포넌트를 등록하면 html파일이나, 다른 컴포넌트에

> <hellow-world>

형식으로 사용할 수 있다.

- **템플릿속성, 스타일 속성**

template, templateUrl : 내부, 외부 파일에 HTML과 템플릿 문법을 이용해 템플릿 정의.

style, styleUrls : 내부, 외부 CSS 파일에 템플릿 스타일 적용.

## 상호작용

- **단방향 바인딩**(클래스->템플릿)

클래스에 선언된 msg 변수가 템플릿 표현식 {{msg}}에 바인딩돼 있으므로 실행시 {{msg}} 위치에 hello가 들어감.

템플릿에서 변경된 내용이, 클래스에 전달되지는 않음.

- **양방향 바인딩**

템플릿에서 변경된 내용이 클래스에도 바인딩.

- **모듈 등록(app.module.ts)**

```typescript
import { MyComponent } from './my.component';
@NgModule({
  declarations : [
    MyComponent
  ],
  ...
})
```

- **부모 컴포넌트 -> 자식컴포넌트 상호작용**

`[]` 기호를 통해 자식 컴포넌트에게 데이터 바인딩가능(컴포넌트로 값 전달)

```typescript
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
```

```typescript
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
```

```typescript
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
```

- **자식 컴포넌트 -> 부모컴포넌트 상호작용**

  EventEmitter의 emit()메서드를 통해 이벤트를 전달.

````typescript
// 자식 컴포넌트
import { Component, EventEmitter, Output } from '@angular/core';

@Component({
  selector: 'child',
  template: `
  <div>자식 <button (click)="updateParent(active)">부모에게 이벤트보내기</button></div>`,
  styles: [`div{border: 2px dotted #666;padding:10px;width:90%;height:50%;}`]
})

export class ChildComponent {
  active = false;
  @Output() outputProperty = new EventEmitter<boolean>();

  updateParent(active: boolean) {
    this.active = !active;
    this.outputProperty.emit(this.active);
  }
}
````

@Output 장식자로 선언한 변수와 동일한 속성명을 이용해 자식 컴포넌트가 전달한 값을 받음.

````typescript
// 부모컴포넌트
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<div>부모<br>
              클릭수 : {{cntClick}}<br>
              자식상태 : {{active}}
              <child (outputProperty)="outputEvent($event)"></child>
          </div>`,
  styles: [`div{border: 2px solid #666;padding:10px;width:400px;height:200px;}`]
})
export class ChildToParentComponent {
  
  cntClick = 0;
  active = false;

  outputEvent(active: boolean) {
    this.cntClick++;
    this.active = active;//자식으로 부터 받은 값
  }
}
````



## 자식 엘리먼트 탐색

- **쉐도우 DOM 한 개의 엘리먼트 가져오기**

지시자는 템플릿 내에 선언돼 있기 때문에(쉐도우 DOM), 초기화 되고 나서 지시자 정보를 얻을 수 있음. 때문에 **@ViewChild와 setTimeout()**함수를 이용해 상태정보를 알아냄.

> @ViewChild(클래스명) 변수명: 클래스명;

````typescript
import {AfterViewInit, Component, Directive, Input, ViewChild} from '@angular/core';

// 지시자의 속성값은 DOM 생성 후 알 수 있음.
@Directive({ selector: 'item' })
export class Item {
  @Input() status: boolean;
}

@Component({
  selector: 'item-component',
  template: '<button>알림창</button>'
})
export class ItemComponent {
  display(){
    alert('ItemComponent입니다');     
  }
}

@Component({
  selector: 'app-view-child',
  template: `
    <item status="false" *ngIf="isShow==false"></item>
    <item status="true" *ngIf="isShow==true"></item>    
    <button (click)="toggle()">선택</button><br>
    isShow : {{isShow}}<br>
    status : {{status}}<br>    
    <item-component (click)="display()"></item-component>`
})
export class ViewchildComponent {

  //@ViewChild를 통해 첫번째 item 엘리먼트를 가져옴.
  @ViewChild(Item)
  set item(v: Item) {    
    setTimeout(() => { this.status = v.status; }, 0);
  }

  @ViewChild(ItemComponent) itemComponent: ItemComponent;
  isShow: boolean = true;
  status: boolean;
 
  toggle() {
    this.isShow = !this.isShow;
  }

  display(){
    this.itemComponent.display();
  } 
}
````

- **쉐도우 DOM 복수개의 엘리먼트 가져오기**

> @ViewChildren('설명 레이블') children:QueryList<참조 변수의 형>;

````typescript
...
<child-cmp #child1 [childname]="'자식1'"></child-cmp>
<child-cmp #child2 [childname]="'자식2'"></child-cmp>
<child-cmp #child3 [childname]="'자식3'"></child-cmp>
...
export class ViewchildrenComponnet {
  @ViewChildren('child1,child2,child3') children:QueryList<ChildCmp>;
}
````

- **DOM의 단수 개의 엘리먼트 가져오기**

  @ContentChild(클래스명) 사용할 변수명: 클래스명;

- **DOM의 복수 개의 엘리먼트 가져오기**

  @ContentChildren(클래스명) 사용할 변수명: 클래스명;

## Reference

쉽고 빠르게 배우는 Angular2 프로그래밍 - 정진욱

@ViewChild와 @ConentChild의 차이 http://stackoverflow.com/questions/34326745/whats-the-difference-between-viewchild-and-contentchild