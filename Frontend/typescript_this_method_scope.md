# Typescript 함수스코프와 this의 이해

Typescript로 코딩을 하다보면, this로 메서드나 함수를 불러오지 못할 때가 있다.

````typescript
class Foo {
    bar = 123;
    bas() {
        console.log(this);
        console.log(this.bar);
    }
}

// 1
var x = new Foo();
x.bas();

// 2
var y = x.bas;
y();
````

1번의 경우는 `this`가 `Foo`이며 `this.bar`의 결과도 올바르게 나오지만,

2번의 경우는 `this`가 `Window`로 나온다. Window는 전역이고, bar는 전역에 있지 않기 때문에, `this.bar`의 결과는 `undefined` 가 나온다.

## 해결책

````typescript
class Foo {
    bar = 123;
    bas = () => {
        console.log(this);
        console.log(this.bar);
    }
}

// 1
var x = new Foo();
x.bas();

// 2
var y = x.bas;
y();
````

위와 같이 ` = () => ` 를 활용하면, this가 `Window`가 아닌 `Foo`가 되며, 결과도 정상적으로 나온다.



## 결론

해당 함수의 실행 스코프가 바뀌어도, `= () =>`를 활용하면, 동일한 스코프를 유지시켜 준다.



## Reference

https://www.youtube.com/watch?v=tvocUcbCupA&hd=1