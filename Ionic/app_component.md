# Ionic2 App Component

- app.component.ts

앱이 최초 실행되는 위치이다. 

```javascript
import { Component } from '@angular/core';
import { HomePage } from '../pages/home/home';

@Component({
  template: '<ion-nav #content [root]="rootPage"></ion-nav>'
})
export class AppComponent {
  rootPage: any = TabsPage;
  ...
}
```

@Component의 메타데이터에, `selector`가 없는 대신 디폴트로 `ion-app`이 사용된다.

`[root]` 는 DOM 속성으로, 어떤 페이지가 RootPage 인지 표시.

**#은 참조변수의 의미**이다. 참조변수가 #content이라고 선언돼 있을때 root 페이지를 참조하려면 {{content.root}}과 같이 접근한다.



- home.ts

앱 컴포넌트에서 참조한 HomePage

````typescript
import { Component } from '@angular/core';
import { NavController } from 'ionic-angular';

@Component({
	selector: 'page-home',
	templateUrl: 'home.html'
})


export class HomePage {
	constructor(public navCtrl: NavController) {
      console.log("시작!")
	}
}
````

- app.module.ts

어떤 페이지, 서비스를 사용하는지, 어떤 컴포넌트가 루트 컴포넌트인지.

````typescript
...
@NgModule({
  declarations: [
    MyApp,
    HomePage,
  ],
  imports: [
    IonicModule.forRoot(MyApp)
  ],
  bootstrap: [IonicApp],
  entryComponents: [
    MyApp,
    HomePage,
    ...
  ],
  providers: [
    ...
    { provide: ErrorHandler, useClass: IonicErrorHandler }
  ]
})
export class AppModule { }
````

@NgModule을 통해 AppModule을 장식

1. declarations : 사용할 컴포넌트들 등록

2. imports: [ IonicModule.forRoot(MyApp) ] : 루트 컴포넌트 등록(MyApp)

3. entryComponents : 페이지 컴포넌트를 등록.

    

## Reference

https://www.youtube.com/watch?v=zp3goVaR8Vg&list=PLAiXlfcSCXYTA8k8AQX0sdRtvpOGkY7yq&index=9