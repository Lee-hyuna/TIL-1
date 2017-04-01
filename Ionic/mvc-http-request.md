## 아이오닉 MVC 패턴으로 json 호출

Angular 2, Ionic 2 기준 MVC 생성



## 모델 생성

src/models 폴더를 생성

- index.ts 파일 생성

````javascript
export * from './[모델명].model';
````

- [모델명].model.ts 파일 생성 : 안에 자료형을 생성해줍니다.

````javascript
export interface [모델명] {
  [변수이름]: [자료형];
  userNo: number;
  startDt: Date;
  timeDivision: string;
  ...
}
````



## 서비스 생성

아이오닉 작업 폴더에서 아래 커맨드 입력

> $ ionic g provider [서비스이름]

그럼 providers 폴더 아래 서비스가 생성된다.

- [서비스파일명].ts : 아래를 참고하여 http 통신 부분을 작성한다.

````javascript
import { Injectable } from '@angular/core';
import { Http, Response } from '@angular/http';
import {Observable} from 'rxjs/Observable'; <!-- Obserable을 사용하여 비동기통신 -->
import 'rxjs/add/operator/map';
import 'rxjs/add/operator/delay';

import {Schedule} from '../models'; <!-- 위에서 생선한 모델명을 {}안에 입력 -->

@Injectable()
export class ScheduleService {
    constructor(public http: Http) {}
    
    getSchedules(): Observable<Schedule[]> {
        return this.http
          .get('http://localhost/schedule/selectScheduleList.do') <!-- 호출한 url 입력-->
          .delay(2000)
          .map((res: Response) => res.json());
    }
}
````

- index.ts

providers 폴더 안에 index 파일을 작성한다.

````javascript
export * from './[추가한 서비스파일명]';
````



##  모듈 추가

- app.module.ts

````javascript
import { NgModule, ErrorHandler } from '@angular/core';
import { IonicApp, IonicModule, IonicErrorHandler } from 'ionic-angular';

import { MyApp } from './app.component';
import { HomePage } from '../pages/home/home';
import { SchedulePage } from '../pages/schedule/schedule';

import { ScheduleService } from '../providers'; <!-- 추가한 서비스경로를 기입 -->

import { StatusBar } from '@ionic-native/status-bar';
import { SplashScreen } from '@ionic-native/splash-screen';

@NgModule({
  declarations: [
    MyApp,
    HomePage,
    SchedulePage
  ],
  imports: [
    IonicModule.forRoot(MyApp)
  ],
  bootstrap: [IonicApp],
  entryComponents: [
    MyApp,
    HomePage,
    SchedulePage
  ],
  providers: [
    ScheduleService, <!-- 추가한 서비스를 기입 -->
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler}
  ]
})
export class AppModule {}
````



## 뷰페이지 작성

- schedule.ts

````javascript
import { Component, OnInit } from '@angular/core'; <!-- OnInit 추가 -->
import { NavController, Refresher, NavParams } from 'ionic-angular'; <!-- Refresher 추가 -->

import {ScheduleService} from '../../providers'; <!-- 서비스와 모델 추가 -->
import {Schedule} from '../../models';

@Component({
  selector: 'page-schedule',
  templateUrl: 'schedule.html'
})
export class SchedulePage implements OnInit {
    schedules: Schedule[] = [];	<!-- json 호출 결과를 담을 변수 추가 -->
    loading: boolean;
    constructor(
        private scheduleService: ScheduleService, <!-- 서비스 생성자에 추가 -->
        public navCtrl: NavController,
        public navParams: NavParams
    ) {}
    
    ngOnInit() {
        this.loading = true;
        const subscription = this.scheduleService.getSchedules().subscribe(schedules => {
          this.schedules = schedules;
          this.loading = false;
          subscription.unsubscribe();
        }, () => this.loading = false);
    }

    doRefresh(refresher: Refresher) {
        const subscription = this.scheduleService.getSchedules().subscribe(schedules => {
          this.schedules = schedules;
          refresher.complete()
          subscription.unsubscribe();
        }, () => refresher.complete());
    }
}
````



- [페이지명].html

```html
<ion-header>
    <ion-navbar>
        <button menuToggle ion-button icon-only>
            <ion-icon name="menu"></ion-icon>
        </button>
        <ion-title>IYLM 메인 페이지</ion-title>
    </ion-navbar>
</ion-header>

<ion-content padding>
    <ion-list>
        <ion-item-group [hidden]="loading">
            <ion-list-header default>
                스케줄 리스트
            </ion-list-header>

            <a ion-item *ngFor="let schedule of schedules">
                <span item-left text-left>{{schedule.schNo}}</span>
                <h2>{{schedule.userNo}}</h2>
                <h2>{{schedule.startDt | date:'y-M-d HH:mm'}}</h2>
                <h2>{{schedule.endDt | date:'y-M-d HH:mm'}}</h2>
                <p>{{schedule.timeDivision}}</p>
                <p>{{schedule.pushTime}}</p>
                <button item-right ion-button icon-only default>
                    detail
                </button>
            </a>

        </ion-item-group>
    </ion-list>
</ion-content>
```



## Reference

[bengtler's Git SampleSource](https://github.com/angularjs-de/ionic2-pizza-service)