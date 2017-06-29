# Typescript Decorator

`tsonfig.json`에서 `"experimentalDecorators": true` 명시해줘야 사용가능

## Class Decorator

````typescript
// 인자가 없을 시엔, 생성자 함수를 선언해줘야
function hello(constructFn: Function) {
    console.log(constructFn);
}

@hello
class Person {
}
````

## Method Decorator

**read only 데코레이터 예제**

함수를 재작성할 수 없게 함.

````typescript
function editable(canBeEditable: boolean) {
    return function (target: any, propName: string, description: PropertyDescriptor) {
        description.writable = canBeEditable;
    }
}

class Person {
    @editable(false)
    hello() {
        console.log('hello');
    }
}

const p = new Person();
p.hello(); // hello
p.hello = function () {
    console.log('world');
}
p.hello(); // hello
````

target/propName/PropertyDescriptor를 파라미터로 받는 함수를 리턴해주어야 함.

## Property Decorator

target/propName을 파라미터로 받는 함수를 리턴해주어야 함.

````typescript
function writable(canBeWritable: boolean) {
    return function (target: any, propName: string): any {
        return {
            writable: canBeWritable
        };
    }
}

class Person {
    @writable(false)
    name: string = "junho";
}

const p = new Person();
console.log(p.name); // junho
p.name = 'dd'; // error!
````

## Parameter Decorator

target/methodName/paramIndex를 파라미터로 가져야함

````typescript
function printInfo(target: any, methodName: string, paramIndex: number) {
    console.log(target);
    console.log(methodName);
    console.log(paramIndex);
}

class Person {
    private _name: string;
    private _age: number;

    constructor(name: string, @printInfo age: number) {
        this._name = name;
        this._age = age;
    }

    hello( @printInfo message: string) {
        console.log(message);
    }
}
````

>  결과
>
> Person {}
>
> hello
>
> 0
>
> [Funcion: Person]
>
> undefined
>
> 1

## Reference

https://www.inflearn.com/course-status-2/