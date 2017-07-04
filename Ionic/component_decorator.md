# Ionic2 Component, Decorator

### Component

W3C 표준인 웹 컴포넌트 기술을 기반으로 HTML, CS, JS를 하나의 단위로 묶어주는 기술로, 앱을 구성하는 요소나 로직을 말함.

### Decorator

Decorator는 주로 클래스, 서비스, 타입, 멤버변수 등을 '장식'하는 역할.

- 클래스 앞에 `@Component`를 해주면 그 클래스는 컴포넌트가 됌.
- 변수 앞에  `@Input` 를 해주면 외부로 부터 값을 받음
- 변수 앞에 `@output`를 해주면 외부로 값을 보냄.

Decorator는 Typescript의 실험적인 구문으로,  `tsconfig.json`의 `experimentalDecorators" : true`를 설정하면 사용할 수 있음(ionic-cli 자동셋팅)

Ionic2에서는 Typescript의 일부 Decorator만 사용 가능, 앵귤러와도 약간 다름.

> ex) 앵귤러의 decorations 등을 아이오닉에서는 사용 불가

### Ionic의 컴포넌트

`@Component 데코레이터`로 장식된 클래스를 컴포넌트라고 통칭함.

### 메타데이터

메타에디터 : `@Component` 에는 하나의 객체 파라미터를 전달할 수 있음 이를 메타데이터라고 함.

````typescript
@Component({
  selector: ... // 컴포넌트를 어디에 표시할지
  template: ... // 컴포넌트 내용을 담는 곳
})
````

