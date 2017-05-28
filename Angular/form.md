# Angular2의 form 전송

````typescript
import {Component} from '@angular/core';
import {NgForm} from '@angular/forms';

@Component({
  selector: 'example-app',
  template: `
    <form #f="ngForm" (ngSubmit)="onSubmit(f)" novalidate>
      <input name="first" ngModel required #first="ngModel">
      <input name="last" ngModel>
      <button>Submit</button>
    </form>
    <p>First name value: {{ first.value }}</p>
    <p>First name valid: {{ first.valid }}</p>
    <p>Form value: {{ f.value | json }}</p>
    <p>Form valid: {{ f.valid }}</p>
  `,
})
export class SimpleFormComp {
  onSubmit(f: NgForm) {
    console.log(f.value);  // { first: '', last: '' }
    console.log(f.valid);  // false
  }
}
````



## css 속성 변화

ng-model에 등록한다면, css 속성이 다음과 같이 변한다.

- ng-touched/ng-untouched : 한 번 포커스가 생겼다가, 없어진 곳은 `ng-touched`
- ng-dirty/ng-pristine : 값 변화시
- ng-valid/ng-invalid : 값 체크



## Reference

https://angular.io/docs/ts/latest/guide/forms.html

https://angular.io/docs/ts/latest/api/forms/index/NgForm-directive.html