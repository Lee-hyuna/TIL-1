# 파이프

템플릿에서 값의 표시되는 형태를 변환해서 보여주기 위해 사용, Filter라는 이름으로 불리기도 함.(|)

- 선언

````typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({ name: 'myupper' })
export class MyUpperPipe implements PipeTransform {
  transform(phrase: string) {
    return phrase.toUpperCase();
  }
}
````

해당 문자열을 대문자로 바꿔주는 파이프.

- 루트 모듈이나, 공유모듈에 등록

`@NgModule({declarations:[MyUpperPipe], exports: [MyUpperPipe]})`

- 사용

````typescript
import { Component } from '@angular/core';

@Component({
  selector: 'player',
  template: `<div highlight>{{"player!!!"|myupper}}</div>`
})
export class PlayerComponent { }
````



## Reference

https://medium.com/witinweb/angular-4-2-pipes-%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-651848186633