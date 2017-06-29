# TypeScript

타입스크립트는 자바스크립트를 확장한 언어. 자바스크립트 **ECMA(European Computer Manufactureres Association) 표준**을 따르고 있기 때문에 ECMA 표준에 대해 알 필요가 있음.

## ECMA 스크립트의 역사

자바스크립트는 ECMA의 표준을 따름. ES1부터 ES7스크립트 표준까지 진행돼옴.

- ES1(ECMA-262 Ed. 1)

  97년 `js`와 `JScript`가 동시에 나오자, ES1이 재정됌.

- ES2

- ES3

  브라우저 지원 시작

- ES4

  불완전한 표준으로, 사용하지 않기로 합의

- **ES5**

  - 현재 자바스크립트가 지원할 수 있는 표준
  - JSON/Accessors
  - 2009 년 Node.js 플랫폼이 등장.(JS가 서버 프로그래밍이 가능한 언어로 확장)

- **ES6**

  - TypeScript의 기반이 되는, 클래스 문법과 모듈 기능 추가
  - IE9 부터 지원

- **ES7**

  - 장식자(decorator)와 Async/Await 특징 추가

![type script에 대한 이미지 검색결과](http://naywinmyint.com/content/images/2016/04/es5-es6-typescript.png)

>  타입스크립트는 ES7 이하의 표준을 포함. 



## 타입스크립트 특징

타입스크립트는 자바스크립트를 확장한 언어로, JS 언어의 특성을 침범하지 않고 최신 ECMA 표준을 지원.

- 컴파일 언어, 정적 타입언어

  - JS는 인터프리터 언어지만, TypeScript는 컴파일언어로 코드 수준에서 미리 타입을 체크하여 오류를 체크해냄. 단 전통적인 컴파일 언어와는 다르게, 링킹 과정이 생략되어 있음.

- 낮은 버전의 ECMA 수용

  - 새로운 표준이 반영되더라도 ES5 브라우저에 대응할 수 있는 타입스크립트 컴파일러를 지원.

  > tsconfig.json 파일의 compilerOptions -> target 필드에 ES 버전 설정 가능

- 타입 기반 언어

  - 타입스크립트 = 자바스크립트 + 타입
  - 컴파일 단계에서 타입 오류를 잡아낼 수 있고, 코드 어시스트 기능도 지원받을 수 있음.
  - 이것은 암묵적 형변환, 호이스팅, 복잡성 문제 해결

- 변환 가능 언어

  - ECMA 표준을 따른다면 컴파일 도구를 이용해 새로운 언어로 마이그레이션 할 수 있도록 지원

## 타입과 문자열

- **let 선언자**

`let 변수명;` 또는`let 변수명 : 타입;`와 같이 사용된다.

**1. 현재 블록내에서만 유효범위를 가지는 변수 선언.** ES6 표준에서 처음 추가된 특징으로, 호이스팅 문제를 해결.

> 호이스팅 문제란?
>
> ````javascript
> var fruit="apple";
> {
>   var fruit="banana";
> }
> console.log(fruit);
>
> > banana
> ````
>
> `{}`안에 선언된 emotion은 지역 스코프이지만, 동일한 이름을 가진 전역 스코프의 변수값을 변경해버리는 문제

2. let은 중복 선언이 불가능. var는 중복선언이 가능.
3. 아래 선언한 변수를 위에서 사용할 수 있음.

- 예제 소스

````javascript
var fruit="apple";
{
  let fruit:string="banana";
  console.log(fruit);
}
console.log(fruit);
> banana
> apple
````

## 배열 타입

- 대괄호 타입

````typescript
let videos: string[] = ["두더지", "인터스텔라", "아바타"];
// or
let videos2: string[] = [];
videos2.push("두더지");
videos2.push("인터스텔라");
videos2.push("아바타");
````

- 제네릭 타입

````typescript
let num:Array<number> = [1, 2, 3];
// or
let num2:Array<number> = new Array<number>();
num2.push(1);
num2.push(2);
num2.push(3);
````

- 다차원 배열

````typescript
var 변수명: 타입[][];
````

## 유니언 타입

타입을 제한하는 기능. `타입 | 타입 | 타입`

- 변수 선언

````typescript
var unionType: string | number = 1;
var unionType2: boolean | string = true;

console.log(typeof unionType, unionType);
console.log(typeof unionType2, unionType2);
> number 1
> boolean true
````

- 매개변수

````typescript
function unionType(p: string | number): string | number {
  return p;
}

let type = unionType(1);
let type2 = unionType("1");

console.log(typeof type, type);
console.log(typeof type2, type2);
> number 1
> boolean 1
````

## 문자열 표현 및 디스트럭처링

문자열을 저장할때  `` ` 을 사용하여 html 태그 `textarea`에 입력하는 것처럼 개행문자를 편하게 입력시킬 수 있음.

`디스트럭처링` 기능을 이용하여, 배열이나 객체를 효율적으로 할당 할 수 있음.

- 소스코드(ES6)

````typescript
var param = ['안녕!!', 33];
let [name, num] = param; // 디스트럭처링 이용해 배열 요소 할당

// 개행 문자 편하게 입력 
console.log(`
메세지 : ${name}
점수 : ${num}
`);

> 메세지 : 안녕!!
  점수 : 33
````

## 화살표 함수

ES6 표준에 포함된 익명함수를 좀 더 간략하게 표현.

````javascript
// 위 익명함수를
function() {
};

// 다음과 같이 표현 가능
() => {}
인자 => 코드

var names = ["남준호", "최종혁", "신예원"];
names = names.forEach(
  (e, index) => console.log(`${index} : ${e}`));
````

## 게터 함수와 세터 함수

get 함수와, set 함수를 만들어 코드를 삽입할 수 있다.

`{set 속성이름(매개변수 선언) { ... }}`

`{get 속성이름() { ... }}`

- **일반적인 JS 코드**

````javascript
var obj = new Object();
obj.message = "lalala";
````

set 과정에 추가 코드를 넣을 수 없다.

- **리터럴 형태 게터함수와 세터 함수**

````typescript
var obj = {
  msg: '',
  get hello() {
    return this.msg + ' world';
  },
  set hello(value) {
    this.msg = value;
  }
};

obj.hello = "hello";
console.log(obj.hello);
> hello world
````

리터럴형태에서는 파라미터 타입을 설정할 수 없다.

- **클래스 형태 게터함수와 세터함수**

````javascript
class Hello {
  private _hello: string;

  // 파라미터 타입 설정 가능
  get hello(): string {
    // world를 더해서 셋팅한다.
    return this._hello + " world";
  }

  set hello(name: string) {
    this._hello = name;
  }
}
hello.hello = 'hello'
console.log(hello.hello);
> hello world
````

## 클래스

- 클래스 선언

````javascript
class MyClass {
  constructor() {
  }
}
var myClass = new MyClass();
var myClass:MyClass = new MyClass();
````

- public 접근자 이용, 생성자 매개변수 전역변수 만들기

````javascript
class MyCar {
  // 퍼블릭으로 선언하면 전역변수로 선언되는 효과
  constructor(public carName: string, public _numTier: number) {}
  getCarName(): string {
    return this.carName;
  }
  get numTier() {
    return this._numTier;
  }
}
````

## 클래스 상속

타입스크립트에서는 상속 기능을 제공

````javascript
class Animal {
  protected constructor(public name: string, public leg: number) {}
  
  getLeg(): number {
    return this.leg;
  }
  protected getName(): string {
    return this.name;
  }
}

class Monkey extends Animal {
  constructor(name: string, leg: number) {
    super(name, leg);
  }
  
  isClimbing() {
    return true;
  }
  
  superGetName() {
    return super.getName();
  }
}
````

## 인터페이스

구현 클래스를 강제함으로써 클래스 형태에 일관성을 유지할 수 있음.

````typescript
interface Animal {
  leg:number;
}

interface Bird extends Animal {
  wing:number;
  getNumWing();
}

class BlueBird implements Bird {
  leg: number;
  wing:number;
  constructor(leg:number, wing:number) {
    this.leg = leg;
    this.wing = wing;
  }
  getNumWing() {
    return this.wing;
  }
}
````

## 추상 클래스

구현과 강제를 동시에 수행하는 클래스, 추상 메서드는 abstract 키워드를 붙여서 형식만 정의, 구현 메서드는 abstract를 생략한 채로 정의

````typescript
abstract class SmallAnimals {
  abstract 추상 메서드(): string;
  구현 메서드(): string {
    // 추상 메서드를 이용해 구현 메서드의 로직 추가
  }
}
````

````typescript
abstract class SmallAnimals {
  abstract sound(): string;
  abstract name(): string;
  makeSound(): string {
    return `${this.name()} : ${this.sound()} `;
  }
}

class Mouse extends SmallAnimals {
  sound(): string {
    return "peep peep~";
  }
  name(): string {
    return "mouse";
  }
}
````



## Reference

쉽고 빠르게 배우는 Angular2 프로그래밍

http://stackoverflow.com/questions/32733230/transpiller-and-internet-explorer-8



