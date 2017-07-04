# Angular2 Form Validate

Form 을 검증하기 위해서는 템플릿 주도인 `내장 검증기`와 모델주도인 `외부 검증모듈`이 있다.

## 템플릿 주도 검증

템플릿 외부에 별도의 검증 모듈을 두지 않고, 템플릿 내에서 검증되게 하는 방법으로 검증대상이 적고 **단순한 검증**을 수행할때 적합한 방식이다.

내장 검증기로는 `required, minlength, maxlength, pattern`가 있다.

````typescript
import { Component } from '@angular/core';

@Component({
  template: `
  	아이디 : <input type="text" [(ngModel)]="user.userId" minlength="2" maxlength="5" #userId="ngModel" required>{{userId.valid}}

  <div *ngIf="userId.valid">
    완료
  </div>`,
  styles: [`
   .ng-touched{ font-weight: bold; }
  `]
})
export class FormComponent {
  user = { userId: '', userName: '' };
}
````

\#userName의 의미는 참조변수이다. input태그의 value값을 가져오기 위해서는 참조변수를 `#userName`과 같이 등록한후 `{{userName.value}}`를 입력하면 된다.

**검증 결과를 확인하기 위해서**는 참조변수에 `#userName = "ngModel"` 을 할당한뒤 `{{userName.valid}}`로 확인할 수 있다.

#### 엘리먼트의 검증상태

위의 {{userName.valid}} 외에도 다른 상태정보를 확인할 수 있다.

1. valid : 검증 통과 true, 실패 false
2. untouched : 포커스를 주지 않았을 경우 true
3. touched : 포커스를 뒀다가 포커스를 잃어버린 경우
4. pristine : 값이 한번도 입력되지 않았을 경우
5. dirty : 값이 한 번이라도 입력된 경우
6. erros : 검증 실패 이유들이 담겨 있음 `{{userName.error | json}}` 으로 확인해보자

#### 폼 참조변수를 이용한 검증

폼 엘리먼트와 연결된 참조변수를 이용해서 검증을 해보자

- NgForm 모듈 필요

`import { NgForm } from '@angular/forms';`

- 폼 엘리먼트와 참조변수 연결

`<form #f="ngForm"> ...` ngForm을 참조변수 #f에 연결한다.

- f.valid를 통해 하나의 변수로 검사 가능

````typescript
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';

@Component({
  template: `
  <form #f="ngForm">
    아이디 :
    <input type="text" name="userId" [(ngModel)]="user.userId" #userId="ngModel" minlength="2" maxlength="5" required>{{userId.valid}}
    비밀번호 :
    <input type="password" name="password" [(ngModel)]="user.userPassword" #password="ngModel" minlength="4" maxlength="12" required>{{password.valid}}
    이름 :
    <input type="text" [(ngModel)]="user.userName" #name="ngModel" [ngModelOptions]="{standalone: true}" required>{{name.valid}}
    <div *ngIf="f.valid">
      완료 {{f.value | json}}, {{f.valid}}
    </div>
  </form>`,
  styles: [`
   .ng-touched{ font-weight: bold; }
  `]
})
export class FormComponent {
  user = { userId: '', userName: '' };
}
````

`{{standalone: true}}`를 하면 f.valid에 영향을 미치지 않으며, name property를 입력하지 않아도 된다.

## 모델 주도 검증

템플릿 외부 모듈에서 검증이 수행된다. **검증과정이 반복적이고 복잡할 경우** 고려한다.

#### 폼 그룹 기반 검증

폼 참조변수는 템플릿 내부에서만 사용할 수 있기 때문에, 클래스와의 연결을 고려한 `폼 그룹 방식`을 사용한다.

`<form [formGroup]="form"> ...`, `<div [formGroup]="form">...`

폼 그룹은 <form> 엘리먼트 외에 엘리먼트의 사용 또한 허용한다. `form` 변수는 클래스 내부에 정의된 FromGroup 객체이다. 폼 그룹을 통해 클래스에서 검증기를 선언할 수 있다.

- Validators.required
- Validators.minLength(5)
- Validators.maxLength(5)
- Validators.pattern("[a-zA-Z]+")

````typescript
import { Component } from '@angular/core';
import { FormGroup, FormControl, Validators } from "@angular/forms";

@Component({
  template: `
  <form [formGroup]="form" (ngSubmit)="onSubmit($event)">
    소문자 : <input formControlName="lowerCase" pattern="[a-z]{3}">
    <br> 대문자 : <input formControlName="upperCase"><br>
    <button type="submit" *ngIf="form.valid">확인</button>
  </form>
  <div>
    <button (click)="setValue()">값 채우기</button>
    <button (click)="reset()">리셋</button>
    <button (click)="patch()">패치</button>
  </div><br>`
})
export class FormComponent {
  form = new FormGroup({
    lowerCase: new FormControl('', Validators.required),
    upperCase: new FormControl('', Validators.compose([Validators.required, Validators.pattern("[A-Z]{3}")]))
  });

  setValue() {
    this.form.setValue({ lowerCase: 'abc', upperCase: 'ABC' });
  }

  reset() {
    this.form.setValue({ lowerCase: '', upperCase: '' });
  }

  onSubmit(event) {
    console.log("this.form.value", this.form.value.lowerCase, this.form.value.upperCase);
    console.log("this.form.value", this.form.get('lowerCase').value, this.form.get('upperCase').value);
  }

  patch() {
    this.form.patchValue({ lowerCase: 'one' });
    this.form.patchValue({ upperCase: 'ONE' });
  }
}
````

두개이상의 Validators를 조합하기 위해서는 compose를 사용한다.

`ngSubmit` 속성을 통해 `onSubmit()` 메서드 호출을 한다. `this.form.get('lowerCase').value` 혹은 `this.form.value.lowerCase`와 같은 방식으로 값을 가져올 수 있다.

#### 커스텀 검증기

복잡한 검증을 수행하기 위해선 커스텀 검증기를 추가해야함. 컴포넌트 외부에 정의함.

- input-validator.ts

````typescript
import { FormControl } from '@angular/forms';

export class InputValidator {
  static startsWithNumber(control: FormControl) {
    var valid = /^\d/.test(control.value);
    if(valid && control.value != "" && !isNaN(control.value.charAt(0))) {
      return { startsWithNumber: true };
    }
    return null;
  }
}
````

#### 폼 검증 상태

`<form [formGroup]="form"> ...`  폼그룹 기반의 검증시에 검증 상태를 알아보자.

- {{form.valid}} : 폼 내부 전체적인 검증상태 확인
- {{Form.touched}} : 폼 전체적인 방문 여부
- {{Form.value | json}} : 폼 내부 값들을 출력해줌
- {{Form.controls.email.value}} : 특정 엘리먼트 속성 값 가져옴
- {{Form.controls.email.touched}}
- {{Form.controls.email.errors | json }}



## Reference

쉽고 빠르게 배우는 Angular2 프로그래밍 - 정진욱