# Javascript Prototype

프로토타입이란, 어떠한 객체가 만들어지기 위해 그 객체의 모태가 되는 원형을 뜻한다.

![img](http://insanehong.kr/post/javascript-prototype/@img/object.jpeg)

 **\_\_proto__** 

A라는 객체를 만들기 위해 사용된 원형, 상위로부터 물려받은 객체의 정보.

**constructor.prototype**

프로토타입 프로퍼티로, 자신을 원형으로 만들어질 다른 객체가 참조할 프로토 타입, 자식에게 물려줄 객체의 정보.



````javascript
//#예제 1.
var A = function () {
    this.x = function () {
         console.log('hello');
    };
};
A.x=function() {
    console.log('world');
};
var B = new A();
var C = new A();
B.x(); // hello
C.x(); // hello
````


````javascript
//#예제 2.
var A = function () { };
A.x=function() {
    console.log('hello');
};
A.prototype.x = function () {
     console.log('world');
};
var B = new A();
var C = new A();
B.x(); // world
C.x(); // world
````
B와 C는 constructor.prototype정보를 바탕으로, 생성이 되기 때문에 예제 2와 같이 prototype을 수정해 주어야 한다.



## Reference

http://insanehong.kr/post/javascript-prototype/