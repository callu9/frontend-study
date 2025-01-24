# 24. 클로저

클로저(closure)는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

## 24.1. 렉시컬 스코프

자바스크립트 엔진은 **함수를 어디에 정의했는지에 따라 상위 스코프를 결정**하는데, 이를 렉시컬 스코프(정적 스코프)라 한다.

**스코프**는 즉 실행 컨텍스트의 렉시컬 환경이다. **스코프 체인**은 렉시컬 환경이 "외부 렉시컬 환경에 대한 참조"를 통해 상위 렉시컬 환경과 연결되는 것을 말한다.

즉, **렉시컬 스코프**는 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장할 참조값, 즉 <ins>상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다</ins>는 것을 의미한다.

## 24.2. 함수 객체의 내부 슬롯 `[[Environment]]`

렉시컬 스코프에서 함수가 정의된 환경, 즉 자신의 상위 스코프를 기억하기 위해 **함수는 자신의 내부 슬롯 `[[Environment]]`에 자신이 정의된 환경(상위 스코프)의 참조를 저장**한다. 이때 저장된 상위 스코프의 참조는 **현재 실행 중인 실행 컨텍스트의 렉시컬 환경**을 가리킨다.

```js
const x = 1;

function foo() {
  const x = 10;

  // 상위 스코프는 함수 정의 환경(위치)에 따라 결정된다.
  // 함수 호출 위치와 상위 스코프는 아무런 관계가 없다.
  bar();
}

// 함수 bar는 자신의 상위 스코프, 즉 전역 렉시컬 환경을 [[Environment]]에 저장하여 기억한다.
function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1
```

아래의 함수 코드 평가 과정 중, 함수 렉시컬 환경의 구성 요소인 **외부 렉시컬 환경에 대한 참조에는 함수 객체의 내부 슬롯 `[[Environment]]`에 저장된 렉시컬 환경의 참조가 할당**된다. 즉, 함수 객체의 내부 슬롯 `[[Environment]]`에 저장된 렉시컬 환경의 참조는 바로 함수의 상위 스코프를 의미한다. 결국 렉시컬 스코프란 <ins>함수 정의 위치에 따라 상위 스코프를 결정</ins>한다는 것이다.

## 24.3. 클로저와 렉시컬 환경

```js
const x = 1;

// ① outer 함수 객체의 생성
function outer() {
  const x = 10;
  
  // ② 중첩 함수 inner의 평가
  const inner = function () { console.log(x); }; 
  return inner;
}

// ③ outer 함수의 호출
// 중첩 함수 inner를 반환하고, 생명주기를 마감 (outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.)
// outer 함수의 렉시컬 환경은 innerFunc에 의해 참조되고 있으므로 소멸하지 않는다.
const innerFunc = outer();

// ④ innerFunc 함수의 실행
innerFunc(); // 10
```

외부 함수보다 중첩 함수가 더 오래 유지되는 경우 **중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다.** 이러한 중첩 함수를 **클로저**(closure)라고 한다.<br />
클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.

- 자유 변수 (free variable): 클로저에 의해 참조되는 상위 스코프의 변수
- 클로저 (closure): 함수가 자유 변수에 대해 닫혀있다. 즉, **자유 변수에 묶여있는 함수**다.

## 24.4. 클로저의 활용

클로저는 **상태를 안전하게 변경하고 유지하기 위해 사용**한다.

즉, 상태가 의도치 않게 변경되지 않도록 **상태를 안전하게 은닉**(information hiding)하고 **특정 함수에게만 상태 변경을 허용**하기 위해 사용한다.

```js
const counter = (function () {
  // 카운트 상태 변수
  let num = 0;

  // 클로저인 메서드를 갖는 객체를 반환한다.
  // 객체 리터럴은 스코프를 만들지 않는다.
  // 따라서 아래 메서드들의 상위 스코프는 즉시 실행 함수의 렉시컬 환경이다.
  return {
    // num: 0, // 프로퍼티는 public하므로 은닉되지 않는다.
    increase() {
      return ++num;
    },
    decrease() {
      return num > 0 ? --num : 0;
    }
  };
}());

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

변수 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적 원인이 될 수 있기 때문에, 외부 상태 변경이나 가변(mutable) 데이터를 피하고 불변성(immutable)을 지향하는 함수형 프로그래밍에서 "부수 효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이기 위해" 클로저가 적극적으로 사용된다.

```js
// 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
const counter = (function () {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;

  // 함수를 인수로 전달받는 클로저를 반환
  return function (aux) {
    // 인수로 전달 받은 보조 함수에 상태 변경을 위임한다.
    counter = aux(counter);
    return counter;
  };
}());

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 보조 함수를 전달하여 호출
console.log(counter(increase)); // 1
console.log(counter(increase)); // 2

// 자유 변수를 공유한다.
console.log(counter(decrease)); // 1
console.log(counter(decrease)); // 0
```

## 24.5. 캡슐화와 정보 은닉

- 캡슐화 (encapsulation): 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것. 정보 은닉의 목적으로 사용하기도 한다.
- 정보 은닉 (information hiding): 객체의 특정 프로퍼티나 메서드를 감추는 것.
  - 외부의 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보를 보호하는 효과
  - 객체 간의 상호 의존성(결합도, coupling)을 낮추는 효과

```js
const Person = (function () {
  let _age = 0; // private

  // 생성자 함수
  function Person(name, age) {
    this.name = name; // public
    _age = age;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

자바스크립트는 접근 제한자(`public`, `private`, `protected`)를 제공하지 않지만, 자유 변수를 통해 `private`한 프로퍼티를 흉내낼 수 있다.

위 예제에서, `Person.prototype.sayHi` 메서드는 즉시 실행 함수가 종료된 이후에 호출되어 소멸하지만, `Person` 생성자 함수와 `sayHi` 메서드가 즉시 실행 함수의 지역 변수 `_age`를 참조할 수 있는 클로저로서 작용한다. 

## 24.6. 자주 발생하는 실수

### 1) 자주 발생하는 실수의 예시
```js
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = function () { return i; };
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]());
}
// 기대한 출력 값 0 1 2
// 실제 출력 값 3
// 전역 변수 i를 참조하고 있기 때문이다.
```

### 2) 즉시 실행 함수를 활용한 개선
```js
var funcs = [];

for (var i = 0; i < 3; i++){
  // 즉시 실행 함수 활용
  funcs[i] = (function (id) { 
    return function () {
      return id;
    };
  }(i));
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // 0 1 2
}

console.log(i) // 3
```

### 3) `let` 키워드 사용을 통한 개선
```js
const funcs = [];

for (let i = 0; i < 3; i++) {
  funcs[i] = function () { return i; };
}

for (let i = 0; i < funcs.length; i++) {
  console.log(funcs[i]()); // 0 1 2
}
```

`let` 키워드로 선언한 변수를 사용하여 `for` 문의 코드 블록이 반복 실행될 때마다 `for` 문 코드 블록의 새로운 렉시컬 환경(LOOP Lexical Environment)이 생성된다. 따라서 `for` 문의 코드 블록 내에서 정의한 함수의 함수의 상위 스코프는 `for` 문의 코드 블록이 반복 실행될 때마다 생성된 `for` 문 코드 블록의 새로운 렉시컬 환경이다.<br />
이때 함수의 상위 스코프는 `for` 문의 코드 블록이 반복 실행될 때마다 식별자(`for` 문의 변수 선언문에서 선언한 초기화 변수 및 `for` 문의 코드 블록 내에서 선언한 지역 변수 등)의 값을 유지해야 한다. 이를 위해 for 문이 반복될 때마다 독립적인 렉시컬 환경을 생성하여 식별자의 값을 유지한다.

### 4) 고차 함수 활용을 통한 개선
```js
// 요소가 3개인 배열을 생성하고 배열의 인덱스를 반환하는 함수를 요소로 추가한다.
// 배열의 요소로 추가된 함수들은 모두 클로저다.
const funcs = Array.from(new Array(3), (_, i) => () => i); // (3) [ƒ, ƒ, ƒ]

// 배열의 요소로 추가된 함수 들을 순차적으로 호출한다.
funcs.forEach(f => console.log(f())); // 0 1 2
```