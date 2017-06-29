# Typescript Generic

다형성을 위해 any를 쓰면, 헬퍼가 제대로 작동하지 않음. Generic을 쓰면 정상 작동함. **Generic**을 쓰자!

````typescript
function helloGeneric<T>(message: T): T {
  return message;
}
console.log(hello<string>('Hello'));
````

1. Generic 타입을 쓰지 않으면 T로 추론.
2. Generic 타입을 쓰면 T를 확인

### Generic Class

````typescript
class Person<T> {
  private _name: T;
  constructor(name: T) {
    this._name = name;
  }
}

const junho = new Person('Junho');
new Person<number>('junho'); // error

class Person<T extends string | number> { ...} // 다음과 같은 식으로 타입제한 가능
````

### Multitype

````typescript
class Person<T, K> {
  name: T;
  age: K;
}

// keyof T는 T의 key(attribute)인지를 확인
function getProperty<T, K extends keyof T> (obj: T, key: K): T[K] {
  return obj[key];
}
````

## Reference

https://www.inflearn.com/course-status-2/