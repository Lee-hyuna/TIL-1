# Typescript Types

타입스크립트의 자료형에 대해 알아보자.

### ECMAScript 표준에 따른 Primitive Type(기본 자료형)

Wrapper 타입과/Primitive Type이 있는데, Primitive Type쓰는 것을 권장함.

- Boolean/boolean
- Number/number
- String/string
- null : 값으로 할당된 것을 null 이라고 함, 모든 자료형에 할당될 수 있음.
- undefined : 값을 할당하지 않은 변수
- Symbol

````typescript
let sym = Symbol();
let obj = {
  [sym]: "value"
};

console.log(obj[sym]); // value
````

- Array : Array<타입>, 타입[]

### 추가 타입

- Any : 최대한 쓰지 않는 것이 TypeScript를 잘쓰는 길이라는 의견이 있음.
- Void(함수 리턴)
- Never(함수 리턴) 
- Enum

````typescript
enum Color { Red, Green=2, Blue}
let c: Color = Color.Green;
console.log(c); // 2
console.log(Color[c]); // Green
````

- Tuple : Array의 변칙 형태로, 배열인데 타입이 한가지가 아닌 경우.

````typescript
const tu : [number, string] = [0, "일"];
````



## Template String

행에 걸쳐있거나, 표현식을 넣을 수 있는 문자열.

## Union Type

string | null | defined 식으로 사용.



## Reference

https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%BD%94%EB%A6%AC%EC%95%84-1705-%EA%B8%B0%EC%B4%88-%EC%84%B8%EB%AF%B8%EB%82%98/