# Prototype
- 자바스크립트에는 클래스라는 개념 대신 **프로토타입** 이라는것이 존재한다.
- 클래스가 없으므로 상속개념도 없는데, 프로토타입으로 상속을 흉내내어 사용한다.

```js
function Human() {
    this.head = 1;
    this.arm = 2;
}

var h1 = new Human();
var h2 = new Human();

console.log(h1.head);  // 1
console.log(h1.arm);  // 2

console.log(h2.head);  // 1
console.log(h2.arm);  // 2
```

h1과 h2는 head와 arm를 공통으로 갖는데, 메모리에는 head와 arm가 두개씩 총 4개가 할당된다.   
객체를 만드는 만큼 메모리에 할당이 된다. 이러한 문제를 프로토타입으로 해결할 수 있다.


```js
function Human(){ }

Human.prototype.head = 1;
Human.prototype.arm = 2;

var h1 = new Human();
var h2 = new Human();

console.log(h1.head); // 1
console.log(h2.head); // 1
```

클래스의 개념과 동일하게 자바스크립트에서는 프로토타입을 활용하여 프로토타입이 갖는 부분을 가져다가 쓸 수 있다.   


# Prototype이 왜 쓰일까?
자바스크립트에는 Prototype Link 와 Prototype Object가 존재한다. 이 둘을 통틀어서 Prototype 이라고 한다.

## Prototype Object
객체는 언제나 함수(Function)로 생성된다.

```js
function Human(){ }        // 함수
var human = new Human();  // 함수로 객체를 생성
```

```js
var obj = {};
```

이 코드도 함수랑 전혀 상관없어 보이지만 이 코드는 다음과 같다.
```js
var obj = new Object();
```

Object가 자바스크립트에서 기본적으로 제공하는 함수이다.   
Function, Array도 모두 함수로 정의되어 있다.   
함수가 정의될 때 2가지 일이 동시에 이루어진다.   

1. **해당 함수에 Constructor(생성자) 자격 부여**
Constructor 자격이 부여되면 new를 통해 객체를 만들 수 있다. 이것이 함수만 new 키워드를 사용할 수 있는 이유이다.


2. **해당 함수의 Prototype Object 생성 및 연결**
함수를 정의하면 함수만 생성되는 것이 아니라 Prototype Object도 같이 생성된다.    
생성된 함수는 prototype 이라는 속성을 통해 Prototype Object에 접근할 수 있다.   
Prototype Object는 일반적인 객체와 같으며 기본적인 속성으로 constructor와 __proto__를 갖는다.   

constructor는 Prototype Object 와 같이 생성되었던 함수를 가리키고 있다.   
__proto__는 Prototype Link이다.( 밑에서 자세히 살펴보자 )








출처 : https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67