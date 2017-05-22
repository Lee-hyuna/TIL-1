# Ionic Component 한줄에 복수개 출력

아이오닉 컴포넌트 체크 박스 4개를 한줄에 출력해보자.

- **html**

````html
<ion-content padding>
  <ion-item class="timeDivision">
    <ion-label>아침</ion-label>
    <ion-checkbox [(ngModel)]="morning"></ion-checkbox>
  </ion-item>
  <ion-item class="timeDivision">
    <ion-label>점심</ion-label>
    <ion-checkbox [(ngModel)]="noon"></ion-checkbox>
  </ion-item>
  <ion-item class="timeDivision">
    <ion-label>저녁</ion-label>
    <ion-checkbox [(ngModel)]="evening"></ion-checkbox>
  </ion-item>
  <ion-item class="timeDivision">
    <ion-label>심야</ion-label>
    <ion-checkbox [(ngModel)]="night"></ion-checkbox>
  </ion-item>
</ion-content>
````

- scss

````scss
.timeDivision {
    width: 23%;
    display: inline-flex;
}
````

