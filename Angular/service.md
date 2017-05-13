# Angular 2.0 Service

컴포넌트에 제공할 목적으로 외부에 정의한 클래스. 모든 컴포넌트에 공통적으로 존재할 수 있는 공통 관심 기능을 서비스로 만듬.

Angular CLI 사용할 경우 

> $ ng g service

## 정의

서비스 정의는 @Injectable 장식자를 사용. 주입 가능한 클래스라는 의미. 서비스 클래스 라는 것을 명시하는 역할.

- 서비스

```typescript
import { Injectable } from '@angular/core';
@Injectable()
export class HelloService {
  sayHello(){
    return "Hello 서비스!";
  }
  constructor() {}
}
```

- 컴포넌트

providers에 서비스를 등록해준다.

````typescript
import { Component } from '@angular/core';
import { HelloService } from './hello.service';

@Component({
  selector: 'hello',
  template: `{{welcome}}`,
  providers: [HelloService]
})
export class HelloComponent {
  welcome: string;

  constructor(helloService: HelloService) {
    this.welcome = helloService.sayHello();    
  }
}
````

## 서비스를 통한 데이터 교환

부모 컴포넌트에는 제공자 설정을 통해 주입. 자식 컴포넌트에는 제공자 설정을 하지 않고 서비스를 받아서 사용하면 같은 서비스를 통해 값 공유 가능.

- 공유 서비스 정의

````typescript
import { Injectable } from '@angular/core';

@Injectable()
export class SharedService {
  num: number;
  message: string;
  public names: string[] = [];
  addName(data: string) {
    this.names.unshift(data);
  }
}
````

- 자식 컴포넌트

````typescript
import { Component, Input } from '@angular/core';
import { SharedService } from './shared.service';

@Component({
  selector: 'car-component',
  template: `car 컴포넌트 : {{ s.message}} <button (click)="s.message='car'">선택</button>`
})
export class CarComponent {
  constructor(public s: SharedService) {
  }
}
````

````typescript
import { Component, Input } from '@angular/core';
import { SharedService } from './shared.service';

@Component({
  selector: 'taxi-component',
  template: `taxi 컴포넌트 : {{ s.message}} <button (click)="s.message='taxi'">선택</button>`
})
export class TaxiComponent {
  constructor(public s: SharedService) {}
}
````

- 부모 컴포넌트

````typescript
import { Component, Input } from '@angular/core';
import { SharedService } from './shared.service';

@Component({
  selector: 'parent-component',
  template: `
  부모 컴포넌트 : {{s.message}} <button (click)="s.message='parent'">선택</button><br>
  <car-component></car-component><br>
  <taxi-component></taxi-component>`,
  providers: [SharedService]
})
export class ParentComponent {
  constructor(public s: SharedService) {
    s.message = "hello";
  }
}
````



## Reference

쉽고 빠르게 배우는 Angular2 프로그래밍 - 정진욱