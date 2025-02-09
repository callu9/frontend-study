# 1. this

## this 키워드

- 설명: 현재 실행 중인 컨텍스트(문맥, 환경)에 따라 다르게 동작하는 키워드입니다.
- 동작 방식
	1. 일반 함수에서 this: **전역 객체**(window)를 가리킴 
	   (use strict에서는 undefined)
		```js
		function show() {
		    console.log(this); // window (브라우저 환경)
		}
		show();
		```

	2. 객체의 메서드에서 this: 해당 객체를 가리킴
		```js
		const obj = {
		    name: "Alice",
		    show: function () {
		        console.log(this.name); // "Alice"
		    }
		};
		obj.show();
		```

	3. 화살표 함수에서 this: 화살표 함수는 this를 부모 스코프에서 가져옴
		```js
		const obj = {
		    name: "Alice",
		    show: () => {
		        console.log(this.name); // undefined (부모 스코프가 window)
		    }
		};
		obj.show();
		```

	4. 생성자 함수에서 this: 새로 생성된 객체를 가리킴
		```js
		function Person(name) {
		    this.name = name;
		}
		const user = new Person("Alice");
		console.log(user.name); // "Alice"
		```

	5. 이벤트 핸들러에서 this: 이벤트를 발생시킨 요소를 가리킴
		```js
		document.querySelector("button").addEventListener("click", function() {
		    console.log(this); // 클릭된 버튼 요소
		});
		```

	6. bind 메소드에서 this: 첫 번째 파라미터로 전달한 특정 객체에 명시적으로 바인딩 됨
		```js
		function returnThis() {
		    return this;
		}
		
		const obj = { name: 'obj' };
		const obj2 = returnThis.bind(obj)();
		
		console.log(obj === obj2); // true
		```

# 2. 렉시컬 스코프

- 스코프: **식별자** 접근 규칙에 따른 **유효 범위**를 뜻한다.
	- 식별자: 변수, 함수 이름과 같이 어떤 대상을 다른 대상과 구분하여 식별할 수 있는 유일한 이름
- 렉시컬 스코프(Lexical scope): 식별자 **유효 범위**가 함수를 호출할 때 결정되는 것이 아닌, **선언**할 때 **결정**되는 것을 뜻한다. 정적 스코프(Static scope)라고도 불리며, 자바스크립트는 렉시컬 스코프를 따른다.

```js
var x = 'foo';

function foo() {
    var x = 'bar';
    bar();
}

function bar() {
    console.log(x);
}

foo(); // foo
bar(); // foo
```

# 3. 프로토타입 체인

## 프로토타입이란

- 프로토타입(Prototype)은 원형이라는 의미를 가진 단어
- 자바스크립트에서 프로토타입은 객체의 원형
- 모든 객체들은 메소드와 속성을 상속 받기 위해 프로토타입 객체를 가진다.
- 즉, 프로토타입 객체란 자신의 부모 역할을 하는 객체를 말한다.

## `[[Prototype]]` , `.__proto__` , `.prototype` 알아보기

 1. `[[Prototype]]` 
 
 개발자가 직접 접근이 불가하고 간접적으로 접근할 수 있는 내부 슬롯으로, 직접 접근을 불가하게 해서 프로토타입 체인의 방향을 자식에서 부모를 탐색하는 단방향으로 지키고, 개발자의 실수로 순환참조하는 일이 없도록 한다.
 - 내부 슬롯이란?자바스크립트 엔진의 구현된 내용을 설명하기 위한 pseudo 프로퍼티
 
```js
function Person(name) {
    this.name = name;
}

console.dir(Person);
```

2. `__proto__` 

객체의 프로토타입(`[[Prototype]]`)을 접근하기 위한 프로퍼티로, 자신의 프로토타입에 접근할 수 있으며, `Object.getPrototypeOf()` 와 동일한 기능을 수행한다.

```js
function Person(name) {
    this.name = name;
}

console.log(Person.__proto__ === Object.getPrototypeOf(Person)); // true
```

3. `prototype` 프로퍼티

생성자 함수로 호출할 수 있는 객체, 즉 constructor 를 소유하는 프로퍼티. 

일반 객체와 생성자 함수로 호출할 수 없는 arrow function은 prototype 프로퍼티가 없다.

```js
function Person(name) {
    this.name = name;
}

// foo 객체를 생성한 객체는 Person() 생성자 함수
const foo = new Person('코드잇');

console.log(foo.__proto__ === Person.prototype); // true
console.log(Person.prototype) // {constructor: f}
```

## 프로토타입 체인

프로토타입 체인은 프로토타입 간의 연결이며, 이를 통해 상위 프로토타입을 검색할 수 있다. 해당 객체에 접근하려는 프로퍼티나 메소드가 없다면 프로토타입(`[[Prototype]]`)이 가리키는 링크, 즉 프로토타입 체인을 따라 자신의 부모 역할을 하는 프로토타입 객체의 프로퍼티나 메소드를 차례대로 검색한다.

모든 객체의 최상위 프로토타입은 `Object.prototype`다.

```js
function Person(name) {
    this.name = name;
}

const foo = new Person('코드잇');
const string = 'abc';
const number = 123;
const array = [1,2,3];
const arrowFunction = () => 'arrowFunction';
const object = {a: 'abc'};

console.log(foo.__proto__ === Person.prototype); // true
console.log(string.__proto__ === String.prototype); // true
console.log(number.__proto__ === Number.prototype); // true
console.log(array.__proto__ === Array.prototype); // true
console.log(arrowFunction.__proto__ === Function.prototype); // true

console.log(foo.__proto__.__proto__ === Object.prototype); // true
console.log(string.__proto__.__proto__ === Object.prototype); // true
console.log(number.__proto__.__proto__ === Object.prototype); // true
console.log(array.__proto__.__proto__ === Object.prototype); // true
console.log(arrowFunction.__proto__.__proto__ === Object.prototype); // true
console.log(object.__proto__ === Object.prototype); // true
```

# 정리
- this가 가리키는 값, 즉 this 바인딩은 실행 컨텍스트에 따라 달라진다.
- 렉시컬 스코프란, 식별자 유효 범위가 함수를 선언할 때 결정된다는 것을 의미한다.
- 프로토타입 체인이란, 객체의 상위 프로토타입과의 연결을 의미하며, 이를 통해 상위 프로토타입의 프로퍼티 혹은 메소드에 접근할 수 있다.