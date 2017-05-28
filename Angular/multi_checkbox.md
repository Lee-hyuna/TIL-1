# Angular multi checkbox

앵귤러2 에서 체크박스를 효율적으로 submit 하는 방법.

- 체크 박스 객체 선언

````typescript
import { NgForm } from '@angular/forms';
...
export class PrescriptionModal {
 ...
  timeDivisions = [
    { id: 'morning', label: '아침', value: 'MORNING', check: false },
    { id: 'noon', label: '점심', value: 'NOON', check: false },
    { id: 'evening', label: '저녁', value: 'EVENING', check: false },
    { id: 'night', label: '밤', value: 'NIGHT', check: false } ];
...
}
````



- 선언한 객체를 ngFor로 반환

````typescript
  <form #prescriptionForm="ngForm" (ngSubmit)="onSubmit(prescriptionForm)">
    <ion-item class="timeDivision" *ngFor="let timeDivision of timeDivisions">
      <ion-label>{{timeDivision.label}}</ion-label>
      <ion-checkbox name="{{timeDivision.id}}" [(ngModel)]="timeDivision.check"></ion-checkbox>
    </ion-item>
    <button>작성 완료</button>
  </form>
````



- 체크 박스 서브밋전 값 편집

````typescript
  onSubmit(prescriptionForm: NgForm) {
    console.log(prescriptionForm.value);
    // 시간 구분을 체크
    this.timeDivisions.forEach(element => {
      if(element.check != false) { // 값 편집
      }
    });
    this.prescriptionService.insertPrescription(this.prescription).subscribe();
  }
````

## Reference

https://stackoverflow.com/questions/37156550/how-to-have-multiple-checkboxes-use-the-same-ng-model

https://forum.ionicframework.com/t/best-way-to-get-checkbox-values/202/3