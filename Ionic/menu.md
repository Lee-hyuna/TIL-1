# 메뉴 만들기

Ionic 2, Anuglar 2 기준에서 새 페이지를 만드는 법

기본 blank 템플릿을 기준으로 추가된 내용을 작성하였다.



## 페이지 만들기

cmd 창에 아이오닉 폴더 안으로 이동후 아래 커맨드를 입력한다.

> $ ionic g page [만들페이지 이름]



그럼 pages 폴더 안에

**페이지명.html, 페이지명.scss, 페이지명, schedule.ts**

세가지 파일이 추가된다.

## 모듈 추가

- app.module.ts

다음과 같이 새로 만든 페이지 내용을 추가한다.

````javascript
import { NgModule, ErrorHandler } from '@angular/core';
import { IonicApp, IonicModule, IonicErrorHandler } from 'ionic-angular';

import { MyApp } from './app.component';
import { HomePage } from '../pages/home/home';
import { SchedulePage } from '../pages/schedule/schedule'; <!-- 추가된 소스 -->

import { StatusBar } from '@ionic-native/status-bar';
import { SplashScreen } from '@ionic-native/splash-screen';

@NgModule({
  declarations: [
    MyApp,
    HomePage,
    SchedulePage <!-- 추가된 소스 -->
  ],
  imports: [
    IonicModule.forRoot(MyApp)
  ],
  bootstrap: [IonicApp],
  entryComponents: [
    MyApp,
    HomePage,
    SchedulePage <!-- 추가된 소스 -->
  ],
  providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler}
  ]
})
export class AppModule {}
````

## 컴포넌트 추가

- app.component.ts 

다음과 같은 내용을 추가한다.

````javascript
import { Component, ViewChild } from '@angular/core'; <!-- 추가된 소스 -->
import { Nav, Platform } from 'ionic-angular';
import { StatusBar } from '@ionic-native/status-bar';
import { SplashScreen } from '@ionic-native/splash-screen';

import { HomePage } from '../pages/home/home';
import { SchedulePage } from '../pages/schedule/schedule';

@Component({
  templateUrl: 'app.html'
})
export class MyApp {
  rootPage:any = HomePage;
  private pages = {}; <!-- 추가된 소스 -->
  @ViewChild(Nav) nav: Nav; <!-- 추가된 소스 -->

  constructor(platform: Platform, statusBar: StatusBar, splashScreen: SplashScreen) {
    platform.ready().then(() => {
      statusBar.styleDefault();
      splashScreen.hide();
    });
    this.pages = { <!-- 추가된 소스 -->
        'HomePage': HomePage,
        'SchedulePage': SchedulePage
    };
  }
  
  // 새 페이지 이동 함수를 추가한다.
  openPage(pageName) {
    const component = this.pages[pageName];
    if (!component) {
      return;
    }
    this.nav.setRoot(component);
  }
}
````



## 메뉴 버튼 만들기

- app.html

````html
<ion-menu [content]="content">
    <ion-header>
        <ion-toolbar>
            <ion-title>Menu</ion-title>
        </ion-toolbar>
    </ion-header>

    <ion-content>
        <ion-list>
            <button ion-item (click)="openPage('HomePage')">
                메인
            </button>
            <button ion-item (click)="openPage('새페이지이름')">
                새페이지이름
            </button>
        </ion-list>
    </ion-content>

</ion-menu>

<ion-nav #content [root]="rootPage"></ion-nav>
````





- home.html / 새페이지.html

````html
<ion-header>
    <ion-navbar>
        <button menuToggle ion-button icon-only>
            <ion-icon name="menu"></ion-icon>
        </button>
        <ion-title>제목</ion-title>
    </ion-navbar>
</ion-header>

<ion-content padding>
	내용
</ion-content>
````



## Reference

[Ionic Doc](http://ionicframework.com/docs/components/#loading)

[bengtler's Git SampleSource]([https://github.com/angularjs-de/ionic2-pizza-service853](https://github.com/angularjs-de/ionic2-pizza-service))