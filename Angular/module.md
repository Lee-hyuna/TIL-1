# Angular Module

모듈은 관련된 기능을 하나로 묶어 다른 코드와 결합도를 줄이고, 재사용성을 높이기 위해 사용된다. 

## Angular 라이브러리 모듈

Angular 라이브러리 모듈의 종류로는 크게 지시자, 파이프, 장식자, 클래스, 인터페이스, Enum, 타입 별칭, 상수가 있음.

- @angular/common :  파이프, 구조 지시자, 속성 지시자 관련 모듈
- @angular/core : 주요 요소 장식자 및 핵심 모듈
- @angular/forms : 폼 관련 모듈, 지시자
- @angular/http : HTTP 통신과 관련된 모듈
- @angular/platform-browser : 브라우저 모듈, DOM 새니타이저 등
- @angular/router : 라우터 관련 모듈이나 지시자
- @angular/testing : 테스팅 관련 모듈

`import { Component} from '@angular/core';` 와 같이 추가.

## 사용자 정의 모듈

```typescript
export class Hi {
  sayHi() {
    return "Hi!";
  }
}

class District {
  id: number; name: string;
}

export var DISTRICT: District[] = [
  { id: 1, name: '서울'}, {id: 2, name: '부산'}, {id: 3, name: '대구'}
];

export function echo(msg:string) {
  return msg;
}
```

export 키워드를 이용해 **모듈을 정의**하고, 모듈을 외부로 노출할 것임을 알림.

- **사용**

```typescript
import { Hi, echo, DISTRICT } from './hi';
```



## Angular 모듈

모듈성이란 성능을 향상 시킬 수 있도록 모듈 간 결합을 최소화하고, 모듈 내부의 응집도는 최대화하는 것을 의미함.

Angular 모듈은 모듈성을 극대화시키기 위해서 **모듈을 그룹단위로 관리**.

- **루트모듈**

  최상위 모듈로서, 컴포넌트, 지시자, 서비스, 파이프와 같은 모듈을 등록하고 관리

- **특징모듈**

  단위 어플리케이션(특정 기능 수행하는 여러 컴포넌트, 서비스 등의 집합)을 구성하는 모듈

  > ex) 게시판 = 단위 어플리케이션 = 리스트컴포넌트 + 글쓰기 컴포넌트 + ...


- **공유 모듈**

  다른 모듈에 포함되어 동작하는 모듈, 모듈 선언시 반복적으로 나타나는 패턴을 묶어 공유모듈로 정의.

- **핵심 모듈**

  항상 동작할 필요가 있거나, 어플리케이션 전체 동작에 핵심적인 역할을 하는 모듈.

  > ex) 타이틀 컴포넌트 : 동작 내내 호출되어야 하기 때문

## Reference

쉽고 빠르게 배우는 Angular2 프로그래밍 - 정진욱

모듈을 이용하여 어플리케이션 구성하기 : http://webframeworks.kr/tutorials/angularjs/app_structure_with_module/