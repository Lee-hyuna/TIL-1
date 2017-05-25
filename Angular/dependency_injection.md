# 의존성 주입

객체 생성과 설정에 들어가는 코드를 최소화하고, 컴포넌트 마다 일관된 방법으로 생성한 객체를 제공하기 위해서 필요함.

1. @Injector : 주입할 클래스 선택
2. Provider : 주입할 클래스를 제공자에 등록
3. Dependency Injection : 생성자로 의존성 주입을 받음

## 제공자를 통한 의존성 주입

- 클래스 제공자

1. `@Injectable()` 장식자 활용 
2. `@Component({ ... providers: [주입할 클래스]})`
3. 생성자 패턴을 이용해 의존성 주입 `constructor(public [주입할 클래스명:클래스])`

- 값 제공자

컴포넌트에 값을 제공할 목적으로 사용.

- 팩토리 제공자

싱글턴이 아닌 새롭게 생성한 객체를 의존성 주입을 통해 사용할 때.

## 제공자 없이 의존성 주입

- 팩토리 패턴

제공자 이용하지 않고 새로운 객체를 만들어 의존성을 주입하고 싶을 때.

- 주입기 이용

Angular에서 만들어 놓은 팩토리 패턴, `new` 키워드 대신 `resolveAndCreate()` 메서드 활용

````typescript
import { ReflectiveInjector } from '@angular/core';
import { 클래스 } from '...';

export function test() {
  let injector: ReflectiveInjector = ReflectiveInjector.resoleveAndCreate([클래스명]);
  return injector.get(클래스명);
}
````

## Reference

쉽고 빠르게 배우는 Angular2 프로그래밍 - 정진욱