# Angular Module

모듈은 관련된 기능을 하나로 묶어 다른 코드와 결합도를 줄이고, 재사용성을 높이기 위해 사용된다. 

## Angular 라이브러리 모듈

Angular 라이브러리 모듈의 종류로는 크게 지시자, 파이프, 장식자, 클래스, 인터페이스, Enum, 타입 별칭, 상수가 있음.

- @angular/common :  파이프, 구조 지시자, 속성 지시자 관련 모듈
- @angular/core : 주요 요소 장식자 및 핵심 모듈
- @angular/forms : 폼 관련 모듈, 지시자
- @angular/http : HTTP 통신과 관련된 모듈
- @angular/platform-browser : 브라우저 모듈, DOM 새니타이저 등
- @angular/router : 라우터 관련 모듈이나 지시자
- @angular/testing : 테스팅 관련 모듈

`import { Component} from '@angular/core';` 와 같이 추가.

## 사용자 정의 모듈

```typescript
export class Hi {
  sayHi() {
    return "Hi!";
  }
}

class District {
  id: number; name: string;
}

export var DISTRICT: District[] = [
  { id: 1, name: '서울'}, {id: 2, name: '부산'}, {id: 3, name: '대구'}
];

export function echo(msg:string) {
  return msg;
}
```

export 키워드를 이용해 **모듈을 정의**하고, 모듈을 외부로 노출할 것임을 알림.

- **사용**

```typescript
import { Hi, echo, DISTRICT } from './hi';
```



## Angular 모듈

모듈성이란 성능을 향상 시킬 수 있도록 모듈 간 결합을 최소화하고, 모듈 내부의 응집도는 최대화하는 것을 의미함.

Angular 모듈은 모듈성을 극대화시키기 위해서 **모듈을 그룹단위로 관리**.

**루트모듈/핵심모듈/특징모듈/공유모듈**로 나누어 관리.

### 루트모듈

최상위 모듈로서, 컴포넌트, 지시자, 서비스, 파이프와 같은 모듈을 등록하고 관리

루트모듈은 @NgModule 장식자를 이용해 정의

````typescript
import { MyComponent } from './my.component';
@NgModule({
  imports: [
  	BrowserModule, CommonModule, FormsModule,
    AppRoutingModule,
    CoreModule.forRoot({nickName: 'Happy'}), // 매개변수 전달
    ...
  ],

  declarations : [
    컴포넌트, 지시자, 파이프,
  ],

  providers: [서비스 모듈, ...]
  ,
  ...
})
````

> imports 속성

- **브라우저 모듈(Browser Module)**

  브라우저 상에서 동작한다면 반드시 포함, 컴포넌트나 지시자, 파이프 같은 구성요소를 템플릿에 나타나게 하는 역할.

- **공통 모듈(Common Module)**

  템플릿에 사용하는 ngIf, ngFor와 관련된 기능 포함(브라우저 모듈에 포함)

- **폼 모듈(FormsModule)**

  NgModel 지시자나 내장 검증기 지시자 등을 포함.

- **라우딩 모듈(RoutingModule)** 

  어플리케이션 수준에서 라우팅 수행

`라우팅 모듈 설정`

````typescript
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { IntroComponent } from './intro.component';
import { HelloComponent } from './hello/hello.component';
import { CoreTestComponent } from './core-test/core-test.component';

const appRoutes: Routes = [
  { path: '', component: IntroComponent },
  { path: 'hello', component: HelloComponent },
  { path: 'lazy', loadChildren: 'app/player/player.module#PlayerModule' },
  { path: 'core-test', component: CoreTestComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(appRoutes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
````

forRoot를 이용해 라우트를 등록, `app.module.ts` 파일 `imports`에 추가

> providers 속성

전역에서 사용할 서비스 등록

> ex) 개발단계에서 전역적으로 사용되는 로거 서비스

> declarations 속성

어플리케이션 레벨에서 사용하고자 하는 컴포넌트, 지시자 파이프

> bootstrap 속성

최상위 컴포넌트인 애플리케이션 컴포넌트 등록

### 핵심 모듈

  항상 동작할 필요가 있거나, 어플리케이션 전체 동작에 핵심적인 역할을 하는 모듈.

  > ex) 타이틀 컴포넌트 : 동작 내내 호출되어야 하기 때문

- **루트 모듈**

`imports`에 핵심모듈 등록, `CoreModule.forRoot({nickName: 'Happy'})`

- **핵심모듈**

```typescript
import { ModuleWithProviders, NgModule, Optional, SkipSelf } from '@angular/core';
import { CommonModule } from '@angular/common';

import { TitleComponent } from './title.component';
import { UserService } from './user.service';
import { UserServiceConfig } from './user.service';

@NgModule({
  imports: [CommonModule],
  declarations: [TitleComponent],
  exports: [TitleComponent],
  providers: [UserService]
})
export class CoreModule {
  constructor( @Optional() @SkipSelf() parentModule: CoreModule) {
    if (parentModule) {
      throw new Error(
        'CoreModule이 이미 로딩되었습니다.');
    }
  }

  static forRoot(config: UserServiceConfig): ModuleWithProviders {
    return {
      ngModule: CoreModule,
      providers: [
        { provide: UserServiceConfig, useValue: config }
      ]
    };
  }
}
```

핵심 모듈을 루트 모듈에 추가하기 위해, `@NgModule` 설정에서 `TitleComponent`를 선언하고, 다시 `export`로 노출

@SkipSelf() : 핵심 모듈이 이미 생성됐는지 검사, 존재 한다면 @Optional 장식자를 이용해 주입

@Optional : 파라미터가 없으면 null 리턴, 있으면 개체 리턴

forRoot : 전달받은 {nickName: 'Happy'} 값이, UserService에서 전달됌.

- **서비스**

````typescript
import { Injectable, Optional } from '@angular/core';

export class UserServiceConfig {
  nickName = '';
}

@Injectable()
export class UserService {
  private _nickName = '';

  constructor( @Optional() config: UserServiceConfig) {
    if (config) { this._nickName = config.nickName; }
  }

  get nickName() {
    return this._nickName;
  }
}
````

forRoot의 파라미터 값으로 UserService 생성자가 실행.

- **타이틀 컴포넌트**

````typescript
import { Component, Input } from '@angular/core';
import { UserService } from '../core/user.service';

@Component({
  selector: 'app-title',
  template: `
  <h1 highlight>{{title}}</h1>
  by <b>{{user}}</b>`
})
export class TitleComponent {
  @Input() title = '';
  user = '';

  constructor(userService: UserService) {
    this.user = userService.nickName;
  }
}
````

- **사용**

````typescript
import { Component } from '@angular/core';

@Component({
  selector: 'core-test',
  template: `
  <app-title [title]="title"></app-title>`
})
export class CoreTestComponent {
  title = '반갑습니다! Core Module!';
}
````


### 특징모듈

  단위 어플리케이션(특정 기능 수행하는 여러 컴포넌트, 서비스 등의 집합)을 구성하는 모듈, 하위 분리하는 모듈.

  > ex) 게시판 = 단위 어플리케이션 = 리스트컴포넌트 + 글쓰기 컴포넌트 + ...

- 특징 모듈 선언

````typescript
import { NgModule }           from '@angular/core';
import { CommonModule }       from '@angular/common';
import { FormsModule }        from '@angular/forms';

import { MemberListComponent }   from './member-list.component';
import { HighlightDirective } from './highlight.directive';
import { MemberRoutingModule }   from './member-routing.module';
import { MemberService }     from './member.service';

@NgModule({
  imports:      [ CommonModule, FormsModule, MemberRoutingModule ],
  declarations: [ MemberListComponent, HighlightDirective ],
  providers:    [ MemberService ]
})
export class MemberModule { }
````

루트 모듈과 유사한 형태로 작성

- 특징 모듈 라우팅 모듈

````typescript
import { NgModule }            from '@angular/core';
import { RouterModule }        from '@angular/router';

import { MemberListComponent }    from './member-list.component';

@NgModule({
  imports: [RouterModule.forChild([
    { path: 'member-list', component: MemberListComponent}
  ])],
  exports: [RouterModule]
})
export class MemberRoutingModule {}
````

- 루트 모듈에 추가

`imports:[ MemberModule, ...]`

### 공유 모듈

자주 사용되는 모듈이지만, 핵심 모듈처럼 항상 사용되는 모듈이 대상이 아닌 특징 모듈을 구성할 때 자주 반복적으로 선언되는 모듈로 반복적으로 나타나는 패턴을 묶어 공유모듈로 정의.

루트 모듈은 특징 모듈을 임포트하고, 특징 모듈은 공유모듈을 임포트함.

- 공유 모델에 사용할 구성요소 정의

````typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({ name: 'myupper' })
export class MyUpperPipe implements PipeTransform {
  transform(phrase: string) {
    return phrase.toUpperCase();
  }
}
````

입력을 대문자로 변환하는 파이프.

- 공유 모듈 설정 파일

````typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';

import { MyUpperPipe } from './my-upper.pipe';

@NgModule({
  imports: [CommonModule],
  declarations: [MyUpperPipe],
  exports: [MyUpperPipe, CommonModule, FormsModule]
})
export class SharedModule { }
````

declarations 선언을 통해 모듈로 등록, 다른 모듈에 포함되어 사용되도록 exports를 통해 다시 외부로 노출]

- 공유모듈 임포트

````typescript
import { NgModule } from '@angular/core';

import { SharedModule } from '../shared/shared.module';

import { PlayerRoutingModule } from './player-routing.module';
import { PlayerComponent } from './player.component';

@NgModule({
  imports: [ PlayerRoutingModule, SharedModule],
  declarations: [PlayerComponent],
  providers: []
})
export class PlayerModule { }
````

이미 공유모듈에 있는 모듈들은 공유모듈안에 있기 때문에, 다시 선언할 필요가 없음. 따라서 모듈 구성에 따른 복잡도를 줄일 수 있음.

## Reference

쉽고 빠르게 배우는 Angular2 프로그래밍 - 정진욱

모듈을 이용하여 어플리케이션 구성하기 : http://webframeworks.kr/tutorials/angularjs/app_structure_with_module/
