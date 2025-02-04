# 25. 클래스

## 25.1. 클래스는 프로토타입의 문법적 설탕인가?

ES6에서 도입된 클래스는 클래스 기반 객체지향 프로그래밍 언어와 매우 흡사한 **새로운 객체 생성 매커니즘**을 제시한다. <br />
이는 프로토타입 기반 객체지향 모델을 폐지한다는 의미는 아니다. 클래스는 함수이며, 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 문법적 설탕(syntatic sugar)이자 새로운 객체 생성 매커니즘이다.

클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다.

1. 클래스를 `new` 연산자 없이 호출하면 에러가 발생하지만, 생성자 함수의 경우 일반 함수로서 호출된다.
2. 클래스는 **상속을 지원하는 `extends`와 `super` 키워드를 제공**하지만, 생성자 함수는 지원하지 않는다.
3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작하지만, 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.
4. 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 해제할 수 없지만, 생성자 함수는 strict mode가 지정되지 않는다.
5. 클래스의 `constructor`, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 `[[Enumerable]]`의 값이 false이므로 열거되지 않는다.

## 25.2. 클래스 정의

`class` 키워드를 사용하여 정의하며, 클래스 이름은 파스칼 케이스를 사용하는 것이 일반적이다. 함수와 마찬가지로 표현식으로 클래스를 정의할 수도 있으며, 이는 클래스가 값으로 사용할 수 있는 일급 객체라는 것을 의미한다.

- [참고] 일급객체로서의 클래스 특징
  - 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
  - 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
  - 함수의 매개변수에게 전달할 수 있다.
  - 함수의 반환값으로 사용할 수 있다.

클래스의 몸체에는 0개 이상의 메서드만 정의할 수 있다.
1. constructor (생성자)
2. 프로토타입 메서드
3. 정적 메서드

```js
class Person { // 클래스 선언문
  // (1) 생성자
  constructor(name) { // 인스턴스 생성 및 초기화
    this.name = name; // name 프로퍼티는 public하다.
  }

  // (2) 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }

  // (3) 정적 메서드
  static sayHello() {
    console.log('Hello!');
  }
}


// 인스턴스 생성
const me = new Person('Lee');

// 인스턴스의 프로퍼티 참조
console.log(me.name); // Lee

// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is Lee

// 정적 메서드 호출
Person.sayHello(); // Hello!
```

## 25.3. 클래스 호이스팅

```js 
// 클래스 선언문
class Person {}

console.log(typeof Person); // function
``` 
클래스는 함수로 평가되기 때문에, 클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정, 즉 런타임 이전에 먼저 평가되어 함수 객체를 생성한다. <br />
이때 클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수, 즉 constructor다. 생성자 함수로서 호출할 수 있는 함수는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다. 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍(pair)로 존재하기 때문이다.

```js
console.log(Person);
// ReferenceError: Cannot access 'Person' before initialization

// 클래스 선언문
class Person {}
```
클래스는 클래스 정의 이전에 참조할 수 없다.

```js
const Person = '';

{
  // 호이스팅이 발생하지 않는다면 ''이 출력되어야 한다.
  console.log(Person);
  // ReferenceError: Cannot access 'Person' before initialization

  // 클래스 선언문
  class Person {}
}
```
클래스 선언문도 변수 선언, 함수 정의와 마찬가지로 호이스팅이 발생한다. 단, 클래스는 `let`, `const` 키워드로 선언한 변수처럼 호이스팅된다. 따라서 클래스 선언문 이전에 일시적 사각지대(TDZ, Temporal Dead Zone)에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작한다.

- [참고] `var`, `let`, `const`, `function`, `function*`, `class` 키워드를 사용하여 선언된 모든 식별자는 호이스팅된다. 모든 선언문은 런타임 이전에 실행되기 때문이다.

## 25.4. 인스턴스 생성

```js
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}

// 클래스를 new 연산자 없이 호출하면 타입 에러가 발생한다.
const you = Person(); // TypeError: Class constructor Person cannot be invoked without 'new'
```
클래스는 생성자 함수이며 `new` 연산자와 함께 호출되어 인스턴스를 생성한다. 함수는 `new` 연산자의 사용 여부에 따라 일반 함수로 호출되거나 인스턴스 생성을 위한 생성자 함수로 호출되지만 클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 반드시 `new` 연산자와 함께 호출해야 한다.

```js
const Person = class MyClass {};

// 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 한다.
const me = new Person();

// 클래스 이름 MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자다.
console.log(MyClass); // ReferenceError: MyClass is not defined
const you = new MyClass(); // ReferenceError: MyClass is not defined
```
클래스 표현식으로 정의된 클래스의 경우, 클래스를 가리키는 식별자(Person)을 사용해 인스턴스를 생성하지 않고 기명 클래스 표현식의 클래스 이름(MyClass)를 사용해 인스턴스를 생성하면 에러가 발생한다. 이는 기명 함수 표현식과 마찬가지로 클래스 표현식에서 사용한 클래스 이름은 외부코드에서 접근 불가능하기 때문이다.

## 25.5. 메서드

클래스의 몸체에는 0개 이상의 메서드만 선언할 수 있으며, constructor (생성자), 프로토타입 메서드, 정적 메서드 세 가지의 메서드를 정의할 수 있다.

### 25.5.1. constructor

constructor는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다. constructor는 이름을 변경할 수 없다.

```js
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```

클래스는 평가되어 함수 객체가 되어, 함수 객체 고유의 프로퍼티를 모두 갖고 있다. 함수와 동일하게 프로토타입과 연결되어 있으며 자신의 스코프 체인을 구성한다.

모든 함수 객체가 가지고 있는 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리키고 있다. 이는 클래스가 인스턴스를 생성하는 생성자 함수라는 것을 의미하여, 즉 `new` 연산자와 함께 클래스를 호출하면 인스턴스를 생성한다.

이때, constructor는 메서드로 해석되는 것이 아니라, 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다. 즉, 클래스 정의가 평가되면 constructor의 기술된 동작을 하는 함수 객체가 생성된다.

- [참고] 프로토타입의 constructor 프로퍼티
  - 프로토타입의 constructor 프로퍼티: 모든 프로토타입이 가지고 있는 프로퍼티이며, 생성자 함수를 가리킨다.

- constructor와 생성자 함수의 차이
  - constructor는 클래스 내에 최대 한 개만 존재할 수 있다. (그렇지 않을 시, 문법 에러(Sysntax Error) 발생)
  - constructor는 생략할 수 있으며, 빈 constructor가 암묵적으로 정의되어 빈 객체를 생성한다.
  - constructor 내부에서 this에 인스턴스 프로퍼티를 추가하면, 프로퍼티가 추가되어 초기화된 인스턴스를 생성할 수 있다.
  - constructor에 매개변수를 선언하고 인스턴스를 생성할 때 초기값을 전달하면, 인스턴스를 생성할 때 클래스 외부에서 인스턴스 프로퍼티의 초기값을 전달할 수 있다.
  - constructor는 별도의 반환문을 갖지 않아야 한다. <br />(생성자 함수와 동일하게 암묵적으로 this, 즉 인스턴스를 반환하기 때문)


### 25.5.2. 프로토타입 메서드

```js
class Person {
  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }
  // 생성자 함수의 경우 Person.prototype.sayHi = function() { ... } 으로 프로토타입 메서드 생성
}

const me = new Person('Lee');

// me 객체의 프로토타입은 Person.prototype이다.
Object.getPrototypeOf(me) === Person.prototype; // → true
me instanceof Person; // → true

// Person.prototype의 프로토타입은 Object.prototype이다.
Object.getPrototypeOf(Person.prototype) === Object.prototype; // → true
me instanceof Object; // → true

// me 객체의 constructor는 Person 클래스다.
me.constructor === Person; // → true
```
클래스 몸체에 정의한 메서드는 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다.<br />
클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 되어 인스턴스의 프로토타입에 존재하는 프로토타입 메서드가 된다. 인스턴스는 프로토타입 메스드를 상속받아 사용할 수 있다.

### 25.5.3. 정적 메서드

```js
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 정적 메서드
  static sayHi() {
    console.log('Hi!');
  }
  // 생성자 함수의 경우 Person.sayHi = function() {...} 으로 정적 메서드 생성
}

// 정적 메서드는 클래스로 호출한다.
// 정적 메서드는 인스턴스 없이도 호출할 수 있다.
Person.sayHi(); // Hi!

// 인스턴스 생성
const me = new Person('Lee');
me.sayHi(); // TypeError: me.sayHi is not a function
```

정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다. 
- [19.12. 정적 프로퍼티/메서드](./19_prototype.md#1912-정적-프로퍼티메서드) 참고

클래스에서는 메서드에 `static` 키워드를 붙이면 정적 메서드(클래스 메서드)가 된다. 클래스는 함수 객체로 평가되므로 자신의 프로퍼티/메서드를 소유할 수 있기 때문에, 클래스 정의(클래스 선언문, 클래스 표현식)이 평가되는 시점에 인스턴스 생성 없이 정적 메서드를 호출할 수 있ㄷ.

정적 메서드는 인스턴스로 호출할 수 없다. 정적 메서드가 바인딩된 클래스는 인스턴스의 프로토타입 체인 상에 존재하지 않기 때문이다. 다시 말해, 인스턴스 프로토타입 체인 상에는 클래스가 존재하지 않기 때문에 인스턴스로 클래스의 메서드를 상속받을 수 없다.

### 25.5.4. 정적 메서드와 프로토타입 메서드의 차이

1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출하고, 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만, 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

```js
class Square {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  // 프로토타입 메서드
  area() {
    return this.width * this.height;
  }
}

const square = new Square(10, 10);
console.log(square.area()); // 100
```
메서드 내부에서 인스턴스 프로퍼티를 참조할 필요가 있다면 this를 사용해야 하며, 이러한 경우 프로토타입 메서드로 정의해야 한다.

```js
class Square {
  // 정적 메서드
  static area(width, height) {
    return width * height;
  }
}

console.log(Square.area(10, 10)); // 100
```
this를 사용하지 않는 메서드는 정적 메서드로 정의하는 것이 좋다.

```js
// 표준 빌트인 객체의 정적 메서드
Math.max(1, 2, 3);          // → 3
Number.isNaN(NaN);          // → true
JSON.stringify({ a: 1 });   // → "{"a":1}"
Object.is({}, {});          // → false
Reflect.has({ a: 1 }, 'a'); // → true
```

### 25.5.5. 클래스에서 정의한 메서드의 특징

1. `function` 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 클래스에 메서드를 정의할 때는 콤마가 필요 없다. (객체 리터럴은 필요)
3. 암묵적으로 strict mode로 실행된다.
4. `for ... in` 문이나 `Object.keys` 메서드 등으로 열거할 수 없다. 즉, 프로퍼티 열거 가능 여부를 나타내는 프로퍼티 어트리뷰트 `[[Enumerable]]`의 값이 false다.
5. 내부 메서드 `[[Construct]]`를 갖지 않는 non-constructor다. 따라서 `new` 연산자와 함께 호출할 수 없다.

## 25.6. 클래스의 인스턴스 생성 과정

1. 인스턴스 생성과 this 바인딩
    - `new` 연산자와 함께 클래스를 호출하면, 클래스가 생성한 인스턴스로서 빈 객체가 생성된다. 
    - 인스턴스의 프로토타입으로 클래스의 prototype 프로퍼티가 가리키는 객체가 설정된다.
    - 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩된다. 따라서 constructor 내부의 this는 클래스가 생성한 인스턴스를 가리킨다.

2. 인스턴스 초기화
    - constructor 내부 코드가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화 한다.
      즉, this에 바인딩된 인스턴스에 프로퍼티를 추가하고, constructor가 인수로 받은 초기값으로 인스턴스의 프로퍼티 값을 초기화 한다.
    - constructor가 생략되었다면 이 과정도 생략

3. 인스턴스 반환
    - 클래스의 모든 처리가 끝나 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

```js
class Person {
  // 생성자
  constructor(name) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // Person {}
    console.log(Object.getPrototypeOf(this) === Person.prototype); // true

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.name = name;

    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  }
}
```

## 25.7. 프로퍼티

### 25.7.1. 인스턴스 프로퍼티

인스턴스 프로퍼티는 constructor 내부에서 정의해야 한다.

```js
class Person {
  constructor(name) {
    // 인스턴스 프로퍼티
    this.name = name; // name 프로퍼티는 public하다.
  }
}

const me = new Person('Lee');

// name은 public하다.
console.log(me.name); // Lee
```

### 25.7.2. 접근자 프로퍼티

접근자 프로퍼티(accessor property)는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수(accessor function)으로 구성된 프로퍼티다.
- [16.3.2. 접근자 프로퍼티](./16_property_attribute.md#1632-접근자-프로퍼티) 참고

```js
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }

  // setter 함수
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(' ');
  }
}

const me = new Person('Ungmo', 'Lee');

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(`${me.firstName} ${me.lastName}`); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
me.fullName = 'Heegun Lee';
console.log(me); // {firstName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(me.fullName); // Heegun Lee

// fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
console.log(Object.getOwnPropertyDescriptor(Person.prototype, 'fullName'));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}
```

클래스의 메서드는 기본적으로 프로토타입 메서드가 된다. 따라서 클래스의 접근자 프로퍼티 또한 인스턴스 프로퍼티가 아닌 프로토타입의 프로퍼티가 된다.

### 25.7.3. 클래스 필드 정의 제안

클래스 필드(class field)는 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리킨다. 

Node.js 12 미만 실행 시 클래스 몸체에 메서드가 아닌 클래스 필드를 선언하면 문법 에러(Syntax Error)가 발생하지만, 최신 브라우저 또는 최신 Node.js(버전 12 이상)에서 실행하면 정상 동작한다.<br />
단, 클래스 몸체에서 클래스 필드를 정의하는 경우 this에 클래스 필드를 바인딩해서는 안 된다. this는 클래스의 constuctor와 메서드 내에서만 유효하며, 클래스 필드를 참조하는 경우에는 this를 반드시 사용해야 한다.


```js
class Person {
  // this에 클래스 필드를 바인딩해서는 안된다.
  this.name = ''; // SyntaxError: Unexpected token '.

  // 클래스 필드 정의 (Class field declarations)
  // 초기화하지 않으면 undefined를 갖는다.
  name = 'Lee';

  constructor(initialName) {
    console.log(name); // ReferenceError: name is not defined
    
    // 클래스 필드 초기화.
    this.name = initialName;
  }

  // 클래스 필드에 함수를 할당 → 인스턴스 메서드가 된다.
  getName = function () {
    return this.name;
  }
  // 화살표 함수로 정의할 수도 있다.
  // getName = () => this.name;
}

const me = new Person('Kim');
console.log(me); // Person {name: "Kim", getName: ƒ}
console.log(me.getName()); // Kim
```
인스턴스를 생성할 때 외부 초기값으로 클래스 필드를 초기화할 필요가 없다면 기존의 constructor에서 인스턴스 프로퍼티를 정의하는 방식과 클래스 필드 정의 제안 모두 사용할 수 있다.

### 25.7.4. private 필드 정의 제안

인스턴스 프로퍼티는 인스턴스를 통해 클래스 외부에서 언제나 참조할 수 있다. 클래스 필드 정의 제안을 사용하더라도 클래스 필드는 기본적으로 public하기 때문에 외부에 그대로 노출된다.

```js
class Person {
  // private 필드 정의
  #name = '';

  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }

  // name은 접근자 프로퍼티다.
  get name() {
    // private 필드를 참조하여 trim한 다음 반환한다.
    return this.#name.trim();
  }
}

const me = new Person('Lee');

// private 필드 #name은 클래스 외부에서 참조할 수 없다.
console.log(me.#name); // SyntaxError: Private field '#name' must be declared in an enclosing class
console.log(me.name); // Lee
```
private 필드는 반드시 클래스 몸체에 정의해야 하며(constructor에 정의하면 에러 발생), 선두에 `#`을 붙여주어야 한다. 참조할 때도 `#`을 붙여야 한다. 단, 클래스 내부에서만 참조할 수 있다.

| 접근 가능성 | public | private |
| --- | --- | --- |
| 클래스 내부 | O | O |
| 자식 클래스 내부 | O | X |
| 클래스 인스턴스를 통한 접근 | O | X |

### 25.7.5. static 필드 정의 제안

static public/private 필드는 최신 브라우저(Chrome 72 이상)과 최신 Node.js(버전 12 이상)에 구현되어 있다.

```js
class MyMath {
  // static public 필드 정의
  static PI = 22 / 7;

  // static private 필드 정의
  static #num = 10;

  // static 메서드
  static increment() {
    return ++MyMath.#num;
  }
}

console.log(MyMath.PI); // 3.142857142857143
console.log(MyMath.increment()); // 11
```

## 25.8. 상속에 의한 클래스 확장

### 25.8.1. 클래스 상속과 생성자 함수 상속

프로토타입 기반 상속은 프로토타입 체인을 통해 다른 객체의 자산을 상속받는 개념이지만, **상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의**하는 것이다.

```js
class Animal {
  constructor(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  eat() { return 'eat'; }

  move() { return 'move'; }
}

// 상속을 통해 Animal 클래스를 확장한 Bird 클래스
class Bird extends Animal {
  fly() { return 'fly'; }
}

const bird = new Bird(1, 5);

console.log(bird); // Bird {age: 1, weight: 5}
console.log(bird instanceof Bird); // true
console.log(bird instanceof Animal); // true

console.log(bird.eat());  // eat
console.log(bird.move()); // move
console.log(bird.fly());  // fly
```

### 25.8.2. extends 키워드

상속을 통해 클래스를 확장하려면 `extends` 키워드를 사용하여 상속받을 클래스를 정의한다.

```js
// 수퍼(베이스/부모)클래스
class Base {}

// 서브(파생/자식)클래스
class Derived extends Base {}
```

- 서브클래스(subclass): 상속을 통해 확장된 클래스. 파생 클래스(derived class) 또는 자식 클래스(child class)라고 부르기도 한다.
- 수퍼클래스(superclass): 서브클래스에게 상속된 클래스. 베이스 클래스(base class) 또는 부모 클래스(parent class)라고 부르기도 한다.

수퍼클래스와 서브클래스는 인스턴스의 프로토타입 체인뿐 아니라 클래스 간의 프로토타입 체인도 생성하기 때문에, 프로토타입 메서드와 정적 메서드 모두 상속 가능하다.

### 25.8.3. 동적 상속

`extends` 키워드를 사용해 생성자 함수를 상속받아 클래스를 확장할 수도 있다. 단, `extends` 키워드 앞에는 반드시 클래스가 와야 한다.

```js
// 생성자 함수
function Base(a) {
  this.a = a;
}

// 생성자 함수를 상속받는 서브클래스
class Derived extends Base {}

const derived = new Derived(1);
console.log(derived); // Derived {a: 1}
```

`extends` 키워드 다음에는 `[[Construct]]` 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다. 이를 통해 동적으로 상속받을 대상을 결정할 수 있다.

```js
function Base1() {}

class Base2 {}

let condition = true;

// 조건에 따라 동적으로 상속 대상을 결정하는 서브클래스
class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived); // Derived {}

console.log(derived instanceof Base1); // true
console.log(derived instanceof Base2); // false
```

### 25.8.4. 서브클래스의 constructor

서브클래스에서 constructor를 생략하면 클래스에 다음과 같은 constructor가 암묵적으로 정의된다. 

```js
constructor(...args) { super(...args); }
```

args는 `new` 연산자와 함께 클래스를 호출할 때 전달한 인수의 리스트다.<br />
`super()`는 수퍼클래스의 constructor(super-constructor)를 호출하여 인스턴스를 생성한다.

```js
// 수퍼클래스
class Base {
  constructor() {}
}

// 서브클래스
class Derived extends Base {
  constructor() { super(); }
}

const derived = new Derived();
console.log(derived); // Derived {}
```

프로퍼티를 소유하는 인스턴스를 생성하려면 constructor 내부에서 인스턴스에 프로퍼티를 추가해야 한다.

### 25.8.5. super 키워드

`super` 키워드는 함수처럼 호출할 수도 있고, this와 같이 식별자처럼 참조할 수 있는 특수한 키워드다.
- `super` 호출 시 수퍼클래스의 constructor를 호출한다.
- `super` 참조 시 수퍼클래스의 메서드를 호출할 수 있다.

```js
// 수퍼클래스
class Base {
  constructor(a, b) { // ④
    this.a = a;
    this.b = b;
  }
}

// 서브클래스
class Derived extends Base {
  constructor(a, b, c) { // ②
    super(a, b); // ③
    this.c = c;
  }
}

const derived = new Derived(1, 2, 3); // ①
console.log(derived); // Derived {a: 1, b: 2, c: 3}
```

- `super` 호출 시 주의사항
  1. 서브클래스에서 constructor를 생략하지 않는 경우 서브클래스의 constructor에서는 반드시 super를 호출해야 한다.
      ```js
      class Base {}

      class Derived extends Base {
        constructor() {
          // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
          console.log('constructor call');
        }
      }

      const derived = new Derived();
      ```
  2. 서브클래스의 constructor에서 `super`를 호출하기 전에는 this를 참조할 수 없다.
      ```js
      class Base {}

      class Derived extends Base {
        constructor() {
          // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
          this.a = 1;
          super();
        }
      }

      const derived = new Derived(1);
      ```
  3. `super`는 반드시 서브클래스의 constructor에서만 호출한다. 서브클래스가 아닌 클래스의 constructor나 함수에서 super를 호출하면 에러가 발생한다.
      ```js
      class Base {
        constructor() {
          super(); // SyntaxError: 'super' keyword unexpected here
        }
      }

      function Foo() {
        super(); // SyntaxError: 'super' keyword unexpected here
      }
      ```

- `super` 참조: 메서드 내에서 super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.
  - 서브클래스의 프로토타입 메서드 내에서 `super.~~`은 수퍼클래스의 프로토타입 메서드를 가리킨다.
  - 서브클래스의 정적 메서드 내에서 `super.~~`은 수퍼클래스의 정적 메서드를 가리킨다.
```js
// 수퍼클래스
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
  
  static sayBye() {
    return 'Bye!';
  }
}

// 서브클래스
class Derived extends Base {
  sayHi() {
    // super.sayHi는 수퍼클래스의 프로토타입 메서드를 가리킨다.
    return `${super.sayHi()}. how are you doing?`;
  }
  static sayBye() {
    // super.sayBye는 수퍼클래스의 정적 메서드를 가리킨다.
    return `${super.sayBye()} see you soon.`;
  }
}

const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi! Lee. how are you doing?
console.log(Derived.sayBye()); // Bye! see you soon.
```

### 25.8.6. 상속 클래스의 인스턴스 생성 과정

#### 1. 서브클래스의 super 호출
서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 인스턴스 생성을 위임하기 때문에, 서브클래스의 constructor에서 반드시 `super`를 호출해야 한다.

#### 2. 수퍼클래스의 인스턴스 생성과 this 바인딩
인스턴스는 `new.target`이 가리키는 서브클래스가 생성한 것으로 처리된다.
    
#### 3. 수퍼클래스의 인스턴스 초기화

#### 4. 서브클래스 constructor로의 복귀와 this 바인딩

`super`가 반환한 인스턴스가 this에 바인딩된다. 서브클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 this에 바인딩하여 그대로 사용한다.

#### 5. 서브클래스의 인스턴스 초기화

#### 6. 인스턴스 반환


```js
// 수퍼클래스
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }

  toString() {
    return `width = ${this.width}, height = ${this.height}`;
  }
}

// 서브클래스
class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
    super(width, height);
    this.color = color;
  }

  // 메서드 오버라이딩
  toString() {
    return super.toString() + `, color = ${this.color}`;
  }
}

const colorRectangle = new ColorRectangle(2, 4, 'red');
console.log(colorRectangle); // ColorRectangle {width: 2, height: 4, color: "red"}

// 상속을 통해 getArea 메서드를 호출
console.log(colorRectangle.getArea()); // 8
// 오버라이딩된 toString 메서드를 호출
console.log(colorRectangle.toString()); // width = 2, height = 4, color = red
```

### 25.8.7. 표준 빌트인 생성자 함수 확장

```js
// Array 생성자 함수를 상속받아 확장한 MyArray
class MyArray extends Array {
  // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
  uniq() {
    return this.filter((v, i, self) => self.indexOf(v) === i);
  }

  // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
  average() {
    return this.reduce((pre, cur) => pre + cur, 0) / this.length;
  }
}

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray); // MyArray(4) [1, 1, 2, 3]

// MyArray.prototype.uniq 호출
console.log(myArray.uniq()); // MyArray(3) [1, 2, 3]

// MyArray.prototype.average 호출
console.log(myArray.average()); // 1.75

console.log(myArray.filter(v => v % 2) instanceof MyArray); // true

// 메서드 체이닝
// [1, 1, 2, 3] => [ 1, 1, 3 ] => [ 1, 3 ] => 2
console.log(myArray.filter(v => v % 2).uniq().average()); // 2
```

`extends` 키워드를 사용하여 표준 빌트인 생성자 함수를 확장하였을 때, 새로운 배열을 반환하는 메서드가 Array의 인스턴스가 아닌 MyArray 인스턴스를 반환하기 때문에 메서드 체이닝(method chaining)이 가능하다.

만약 MyArray 클래스의 매서드가 Array 인스턴스를 반환하게 하려면 다음과 같이 `Symbol.species`를 사용하여 정적 접근자 프로퍼티를 추가한다.

```js
// Array 생성자 함수를 상속받아 확장한 MyArray
class MyArray extends Array {
  // 모든 메서드가 Array 타입의 인스턴스를 반환하도록 한다.
  static get [Symbol.species]() { return Array; }

  // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
  uniq() {
    return this.filter((v, i, self) => self.indexOf(v) === i);
  }

  // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
  average() {
    return this.reduce((pre, cur) => pre + cur, 0) / this.length;
  }
}

const myArray = new MyArray(1, 1, 2, 3);

console.log(myArray.uniq() instanceof MyArray); // false
console.log(myArray.uniq() instanceof Array); // true

// 메서드 체이닝
// uniq 메서드는 Array 인스턴스를 반환하므로 average 메서드를 호출할 수 없다.
console.log(myArray.uniq().average()); // TypeError: myArray.uniq(...).average is not a function
```