# Typescript Interface

### Interface

````typescript
interface Person {
  name: string;
  age?: number; // '?' 있을 수도 있고 없을 수도 있음.
  [index: string]: string; // indexable type : 프로퍼티 명으로 옵셔널하게 사용가능
  [index: number]: number;
  hello(): void;
  hi(): string;
  ho(): string;
}

const person: Person = {
  name: 'Junho',
  age: 35,
  hello: function() {},
  hi: (): string => { return 'hi';  },
  ho(): string { return 'hello'; }
};

person.anybody = "junho"; // indexable type 사용
person[0] = 1;

function hello(p: Person): void {
  console.log('안녕하세요 ${p.name} 입니다.');
}
````



### Class Implements Interface

````typescript
interface IPerson {
  name: string;
  hello(): void;
}

class Persion implements IPerson {
  name: string = null;
  hello(): void {
    console.log('안녕하세요 ${this.name} 입니다.');
  }
  
  constructor(name: string) {
    this.name = name;
  }
  
  public hi() : void {
    console.log('안녕 준호임');
  }
}

const person: Person = new Person('Junho');
person.hi();
````

### Function Interface

````typescript
interface HelloPerson {
  (name: string, age?: number): void;
}

let helloPerson: HelloPerson = function(name: string) {
  console.log(name);
}

helloPerson('Junho');
````

함수인터페이스는 사용할때 타입체크를 함.



## Reference

https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%BD%94%EB%A6%AC%EC%95%84-1705-%EA%B8%B0%EC%B4%88-%EC%84%B8%EB%AF%B8%EB%82%98/?subscribe