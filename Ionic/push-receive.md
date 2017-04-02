## 아이오닉 Push 수신

서버에서 아이오닉 앱으로 Push 메세지 수신기능 구현.

구글 FCM을 활용한다. (Ionic 2, Angular 2 기반)



## Cloud Client 셋팅

아이오닉 서비스와 통신하기 위해서 Cloud Client 셋팅이 필요

> $ npm install @ionic/cloud-angular --save



## App Id

> $ ionic to init

위 명령어를 치면 **ionic.config.json**에 APP_ID 가 셋팅된다.

이 APP_ID를 기억할것.



## 설정

- **src/app/app.module.ts**

위 파일에 아래와 같이 클라우드 셋팅 정보를 추가한다.

APP_ID에 위에서 메모한 정보를 입력

```javascript
import { CloudSettings, CloudModule } from '@ionic/cloud-angular';

const cloudSettings: CloudSettings = {
  'core': {
    'app_id': 'APP_ID'
  },
  'push': {
    'sender_id': 'SENDER_ID',
    'pluginConfig': {
      'ios': {
        'badge': true,
        'sound': true
      },
      'android': {
        'iconColor': '#343434'
      }
    }
  }
};

@NgModule({
  declarations: [ ... ],
  imports: [
    IonicModule.forRoot(MyApp),
    CloudModule.forRoot(cloudSettings)
  ],
  bootstrap: [IonicApp],
  entryComponents: [ ... ],
  providers: [ ... ]
})
export class AppModule {}
```



## 안드로이드 FCM ID 발급

구글 Firebase로 이동해 Fcm ID를 발급받는다.

프로젝트 생성 후.

Settings->Cloud Messageing Tab에서 확인할 수 있다.

[Firebase](https://console.firebase.google.com/)



## Push Plugin 설치

위에서 발급받은 Sender_ID를 포함하여 아래 명령어를 입력

> $ cordova plugin add phonegap-plugin-push --variable SENDER_ID=12341234 --save



## Push 등록/메세지 수신 코드

````javascript
import { Component, OnInit } from '@angular/core';
import { NavController, NavParams, Platform } from 'ionic-angular';
import { Push, PushToken } from '@ionic/cloud-angular';

@Component({
    selector: 'page-schedule',
    templateUrl: 'schedule.html'
})
export class SchedulePage implements OnInit {
    constructor(
        public navCtrl: NavController,
        public navParams: NavParams,
        public push: Push,
        public platform: Platform
    ) {}

    ngOnInit() {
        if (this.platform.is('android')) {
            // 푸시 Register
            this.push.register().then((t: PushToken) => {
                return this.push.saveToken(t);
            }).then((t: PushToken) => {
                console.log('Token saved:', t.token);
            });
    
            // 푸시 메세지 수신
            this.push.rx.notification()
                .subscribe((msg) => {
                    alert(msg.title + ': ' + msg.text);
            });   
        } else {
            console.log('not android');
        }
    }
}
````

푸시를 등록할 페이지에 위와 같이 코드를 등록한다.

웹에서 테스트하면, push not found... 와 같은 에러메세지가 나오므로

에뮬레이터나 실제 디바이스에서 테스트 해야한다.



푸시 Register 부분은 로그인 이후에 넣는 것이 권장된다.



## 푸시 수신 테스트(안드로이드)

[대시보드](https://apps.ionic.io/apps/)

대시보드에서 앱설정으로 들어간 뒤

Settings -> Certificates 에서 프로필을 생성한다.



다음 생성된 프로필에 'Edit'에 들어간뒤

키를 넣어야 하는데.



MY-RELEASE-KEY : 키 파일 이름

MY_ALIAS_NAME : 식별 이름 

위 내용을 본인에 맞게 수정 후 커맨드를 입력한다.

> ```
> $ keytool -genkey -v -keystore MY-RELEASE-KEY.keystore -alias MY_ALIAS_NAME -keyalg RSA -keysize 2048 -validity 10000
> ```



키 생성을 위한 정보를 입력하고 비밀번호를 입력하면.

프로젝트 root 폴더에 키가 생성된다.



해당 키파일과 비밀번호를 위의 Ceftificates -> Profile -> Edit

에 등록한다.



등록 후 DashBoard -> 프로젝트 선택 -> Push 탭에서

푸시메세지를 발송해볼 수 있다.



**에뮬레이터나, 실제 디바이스에서 테스트 해볼것



## Reference

[Ionic Doc Push](http://docs.ionic.io/services/push/)