# export class와 default export class

### default export

````typescript
// MyClass.ts
export default class MyClass { /* ... */ }
````

export defulat의 경우, 파일당 하나의 default export만 가능하다.

````typescript
import MyClass from "./MyClass";
````

다음과 같이 rename을 해줄 수도 있다.

````typescript
import MyClassAlias from "./MyClass";
````



### named export

````typescript
// MyClass.ts
export class MyClass { /* ... */ }
export class MyOtherClass { /* ... */ }
````

export의 경우, 다중 export가 가능하다, 이름도 지정 가능.

````typescript
import {MyClass, MyOtherClass as MyOtherClassAlias} from "./MyClass";
````



### 어떤 것을 사용할까?

프로젝트가 복잡해질 수록, named export가 유용하다는 의견이 있다. default export의 경우 해당 파일에서만 유효하지만, named export의 경우는 다른 파일들의 클래스에서도 유효하다고 한다.



## Reference

https://stackoverflow.com/questions/33305954/typescript-export-vs-default-export