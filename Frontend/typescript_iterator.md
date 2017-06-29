# Typescript iterator

Symol.iterator 함수가 구현되어 있으면 iterable 이라고 함.`implements iterable<T>`

Array, Map, Set, String.. 등

````typescript
class CustomIterable implements Iterable<string> {
    private _array: Array<string> = ['first', 'second'];

    [Symbol.iterator]() {
        var nextIndex = 0;

        return {
            next: () => {
                return {
                    value: this._array[nextIndex++],
                    done: nextIndex > this._array.length
                }
            }
        }
    }
}

const cIterable = new CustomIterable();

for (const item of cIterable) {
    console.log(item);
}
````

## for..in

**배열을 순회할 때는 사용하지 않을 것**

루프가 무작위로 순회할 수도 있음, index가 number이 아닌 string으로 나옴

**객체를 순회할 때** 쓰세요.

## for..of

배열을 순회할때 사용.

객체를 순회할때도, `for (const item of Object.keys(객체)) {..}`방식으로 사용 가능

````typescript
class Person {
  _name: string = null;
  _age: number = null;

  constructor(name: string, age: number) {
      this._age = age;
      this._name = name;
  }
};

const junho = new Person('Junho', 12);

for (const item in junho) {
    console.log(item);
    console.log(junho[item]);
}

for (const item of Object.keys(junho)) {
    console.log(item);
    console.log(junho[item]);
}
````

## Reference

https://www.inflearn.com/course-status-2/