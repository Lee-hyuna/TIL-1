# 라우터

사용자가 요청한 URL을 해석하고 출력을 담당하는 컴포넌트와 연결하는 역할. 

- 기본적으로 `<router-outlet></router-outlet>` 영역에 출력.

## 라우터 지시자

- a 태그

href 속성은 애플리케이션 전체를 로딩하기 때문에 routerLink를 사용함.

`<a routerLink="/pages/first-page">1</a>`

- 네비게이션

````typescript
import { Router } from '@angular/router';

// 생성자 매개변수 public 사용시, 클래스 전역 변수가 됌.
constructor(public _router: Router) { } 

// 페이지 이동
this._router.navigateByUrl("/pages/first-page");
// 또는
this._router.navigate(['pages', 'second-page']);
````

## 라우터 골격

- 라우터 설정

````typescript
import { ModuleWithProviders } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { IntroComponent } from './intro.component';
import { NotFoundComponent } from './not-found.component';
import { HelloComponent } from './hello/hello.component';

const helloRoutes: Routes = [
  // 라우터 설정 정보
  { path: '', component: IntroComponent },
  { path: 'hello', component: HelloComponent }
];

const appRoutes: Routes = [
  // 라우터 설정 변수
  ...helloRoutes,
  { path: '**', component: NotFoundComponent,
    path: 'lazy', loadChildren: 'app/player.module#PlayerModule'
  }
];

export const appRoutingProviders: any[] = [
  ...
];

export const AppRoutingModule: ModuleWithProviders = RouterModule.forRoot(appRoutes);
````

`Routes`는 라우터 설정의 구조를 정의한 인터페이스 모듈

`RouterModule`은 지시자나 컴포넌트를 포함해 모듈을 만들 때 사용하는 모듈로 `RouterModule.forRoot(...)` 메서드는 appRoutes의 라우터 설정 정보에 담긴 지시자나 컴포넌트의 정보를 합해서, 어플리케이션 단위의 모듈로 만드는 역할.

`appRoutes`에서 여러 라우터 설정 정보를 합치려면 `...` 와 같은 절개 연산자를 이용하면 됌.

`AppRoutingModule`은 루트 모듈에 포함돼야 하기 때문에 `export` 처리를 함.

`loadChildren` 게으르게 임포트함.

- 루트 모듈

````typescript
...
/* application router settings */
import { AppRoutingModule, appRoutingProviders } from './app.routing';

/* global components */
import { AppComponent } from './app.component';
import { IntroComponent } from './intro.component';
import { HelloComponent } from './hello/hello.component';

@NgModule({
  imports: [
    ...
    AppRoutingModule
  ],
  providers: [appRoutingProviders],
  declarations: [
    ...
    AppComponent, IntroComponent, HelloComponent,
    NotFoundComponent
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
````

- 어플리케이션 컴포넌트(app.component.ts)

````typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    ...
    <router-outlet></router-outlet>`
})
export class AppComponent { }
````

`<router-outlet></router-outlet>` 라우터 아울렛을 반드시 포함.

- 라우터로 연결되는 컴포넌트

````typescript
import { Component } from '@angular/core';

@Component({
  selector: 'hello',
  template: `<h1>Hello!!</h1>`
})
export class HelloComponent { }
````

## 특징 모듈에서 라우팅 설정

위의 코드는 루트 모듈에서만 사용된다. 루트 모듈은 맨 처음 한번 시작 포인트를 갖기 때문에, lazy load와 같은 어플리케이션이 시작 시점에 초기화 되지 않는 모듈일 경우에는 오류가 발생한다. 그래서 이러한 점을 대처하기 위해 `RouterModule.forChild`를 활용한다.

- **forChild()를 이용한 라우팅 설정**

````typescript
import { NgModule }            from '@angular/core';
import { RouterModule }        from '@angular/router';

import { MemberComponent }    from './member.component';

@NgModule({
  imports: [RouterModule.forChild([
    { path: 'member', component: MemberComponent}
  ])],
  exports: [RouterModule]
})
export class MemberRoutingModule {}
````

- **라우팅 설정을 특징 모듈에 추가**

````typescript
import { NgModule }           from '@angular/core';
import { CommonModule }       from '@angular/common';
import { FormsModule }        from '@angular/forms';

import { MemberComponent }   from './member.component';
import { MemberRoutingModule }   from './member-routing.module';

@NgModule({
  imports:      [ CommonModule, FormsModule, MemberRoutingModule ],
  declarations: [ MemberComponent ],
  providers:    [ ]
})
export class MemberModule { }
````

## 라우팅 접근 제어

라우팅 접근제어에 **가드(guard)**를 사용, 크게 4가지 종류가 있음

- CanActivate 가드, CanActivateChild 가드

라우터의 접근 권한을 검사, CanActivate 인터페이스 모듈을 받아 canActivate() 메서드를 구현. url을 체크하고 참이나 거짓 값중 하나를 반환함.

- CanDeactive 가드

라우트 변경시 호출 되는 라우트.

- Resolve 가드

라우트 데이터를 가져와 컴포넌트에 제공하는 가드.(주소에서 id를 가져온다든지)

- CanLoad 가드

lazy 모듈 임포트시 사용되는 가드.

````typescript
const lazyRoutes: Routes = [
  {
    path: 'lazy',
    loadChildren: 'app/player/player.module#PlayerModule',
    canLoad: [AuthGuard]
  }
];
````

## Reference

쉽고 빠르게 배우는 Angular2 프로그래밍 - 정진욱

https://stackoverflow.com/questions/40498081/routermodule-forrootroutes-vs-routermodule-forchildroutes