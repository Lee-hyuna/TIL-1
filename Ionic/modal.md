# 모달(Modal)

모달 추가 방법 정리

## 모달 호출

- 호출 컴포넌트

````typescript
...
import { ModalController } from 'ionic-angular';

// 이번의 주인공 모달 페이지 추가
import { DrugboxModal } from '../drugbox/drugbox';

export class HomePage implements OnInit {
  ...
  	constructor(
		...
		public modalCtrl: ModalController
    ) { ... }
  
  	openDrugboxModal() {
		const modal = this.modalCtrl.create(DrugboxModal);
		modal.present();
	}
}
````

- 템플릿

````html
<div class="row row-center">
  <div class="col col-center">
    <button ion-button block color="danger" (click)=openDrugboxModal()><ion-icon name="archive"></ion-icon>
      약물 등록
    </button>
  </div>
</div>
````

## 모달

- 모달 컴포넌트

````typescript
import { Component } from '@angular/core';
import { ViewController } from 'ionic-angular';

@Component({
  templateUrl: 'drugbox.html',
})
export class DrugboxModal {
  constructor(private viewCtrl: ViewController) {}

  closeModal(): void {
    this.viewCtrl.dismiss();
  }
}
````

- 모달 템플릿

````html
<ion-header>
  <ion-toolbar>
    <ion-buttons start>
      <button (click)="closeModal()" ion-button icon-only>
        <ion-icon name="close"></ion-icon>
      </button>
    </ion-buttons>
    <ion-title>약품함</ion-title>
  </ion-toolbar>
</ion-header>

<ion-content padding>약품함입니다.</ion-content>
<ion-footer>
  <ion-toolbar>
    <small>약품을 등록해주세요!</small>
  </ion-toolbar>
</ion-footer>
````



## 모달 등록

- app.module.ts

````typescript
...
import { DrugboxModal } from '../pages/drugbox/drugbox';
...

@NgModule({
  declarations: [
    ...
    DrugboxModal
  ],
  ....
    entryComponents: [
    ...
    DrugboxModal
  ],
  ....
````

## Reference

https://github.com/angularjs-de/ionic2-pizza-service/blob/master/src/pages/order/order.component.ts