# 12. 함수

## 12.1. 함수란?
프로그래밍 언어의 함수는 **일련의 과정을 문(statement)으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것**이다. 
- parameter (매개 변수): 함수 내부로 입력을 전달받는 변수
- argument (인수): 함수 입력 값
- return value (반환값): 함수 출력 값

```js
// 함수 정의
function 함수이름(매개변수) {
  // 함수 몸체
  return 반환값;
}

// 함수 호출
함수이름(인수);
```

함수는 값이며 여러 개 존재할 수 있으므로 특정 함수를 구별하기 위해 식별자인 함수 이름을 사용할 수 있으며, **함수 정의**(function definition)를 통해 생성한다. 미리 정의된 함수에 필요한 입력, 즉 인수를 매개변수를 통해 함수에 전달하면서 함수의 실행을 명시적으로 지시함으로써 **함수 호출**(function call/invoke)한다. 함수를 호출하면 코드 블록에 담긴 문들이 일괄적으로 실행되고, 실행 결과, 즉 반환값을 반환한다.

## 12.2. 함수를 사용하는 이유

동일한 작업을 반복적으로 수행해야 할 경우 미리 정의된 함수를 재사용함으로써 효율성을 가질 수 있다.

- 코드의 재사용 측면에서 용이
- 유지보수의 편의성 향상
- 코드의 신뢰성 향상
- 코드의 가독성 향상

## 12.3. 함수 리터럴

함수 리터럴은 `function` 키워드, 함수 이름, 매개 변수 목록, 함수 몸체로 구성되며, 평가되어 값을 생성하는 객체다. 일반 객체와 다르게 함수는 호출할 수 있는 객체이며, 일반 객체에는 없는 함수 객체만의 고유한 프로퍼티를 갖는다. ([18. 함수와 일급 객체](./18_function_and_first_class_object.md) 참고)

```js
// 변수에 함수 리터럴을 할당
var f = function add(x, y) {
  return x + y;
};
```

#### 함수 리터럴의 구성 요소

- 함수 이름
  - 함수 이름은 식별자이므로, 식별자 네이밍 규칙을 준수해야 한다.
  - 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자다.
  - 함수 이름은 생략할 수 있다. 이름이 있는 함수를 기명 함수(named function), 이름이 없는 함수를 무명/익명 함수(anonymous function)이라 한다.
- 매개변수 목록
  - 0개 이상의 매개변수를 소괄호로 감싸고 쉼표로 구분한다.
  - 각 매개변수에는 함수를 호출할 때 지정한 인수가 순서대로 할당된다. 즉, 매개변수 목록은 순서에 의미가 있다.
  - 매개변수는 함수 몸체 내에서 변수와 동일하게 취급되므로, 식별자 네이밍 규칙을 준수해야 한다.
- 함수 몸체
  - 함수가 호출되었을 때 일괄적으로 실행될 문들을 하나의 실행 단위로 정의한 코드 블록이다.
  - 함수 몸체는 함수 호출에 의해 실행된다.


## 12.4. 함수 정의

- 함수 정의: 함수를 호출하기 이전에 인수를 전달받을 매개변수와 실행문들, 반환값을 지정하는 것. 정의된 함수는 자바스크립트 엔진에 의해 평가되어 함수 객체가 된다.

### 12.4.1. 함수 선언문

함수 선언문은 표현식이 아닌 문으로, 함수 리터럴과 형태가 동일하나 함수 이름을 생략할 수 없다는 차이점이 있다. 자바스크립트 엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하여 함수 객체를 할당한다. 즉, 함수 이름으로 호출하는 것이 아니라 함수 객체를 가리키는 식별자로 호출한다.

```js
// 함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 참조
// console.dir은 console.log와는 달리 함수 객체의 프로퍼티까지 출력한다.
// 단, Node.js 환경에서는 console.log와 같은 결과가 출력된다.
console.dir(add); // ƒ add(x, y)

// 함수 호출
console.log(add(2, 5)); // 7

// 함수 선언문은 함수 이름을 생략할 수 없다.
// SyntaxError: Function statements require a function name
function (x, y) {
  return x + y;
}
```

### 12.4.2. 함수 표현식

자바스크립트의 함수는 일급 객체(값의 성질을 갖는 객체)이므로, 함수 리터럴로 생성한 함수 객체를 변수에 할당할 수 있다. 이러한 함수 정의 방식을 함수 표현식(function expression)이라 한다.

함수 표현식은 "표현식인 문", 함수 선언문은 "표현식이 아닌 문"이라는 차이가 있다.

```js
// 기명 함수 표현식
var add = function foo (x, y) {
  return x + y;
};

// 함수 객체를 가리키는 식별자로 호출
console.log(add(2, 5)); // 7

// 함수 이름으로 호출하면 ReferenceError가 발생한다.
// 함수 이름은 함수 몸체 내부에서만 유효한 식별자다.
console.log(foo(2, 5)); // ReferenceError: foo is not defined
```

### 12.4.3. 함수 생성 시점과 함수 호이스팅

함수 선언문으로 정의한 함수는 함수 선언문 이전에 호출할 수 있으나, 함수 표현식으로 정의한 함수는 함수 표현식 이전에 호출할 수 없다. 이는 함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 **함수의 생성 시점이 다르기 때문**이다.

```js
// 함수 참조
console.dir(add); // ƒ add(x, y)
console.dir(sub); // undefined

// 함수 호출
console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // TypeError: sub is not a function

// 함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 표현식
var sub = function (x, y) {
  return x - y;
};
```

- 함수 호이스팅 (function hoisting)
  - 함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징
  - 코드가 한 줄씩 순차적으로 실행되는 런타임 이전에, 함수 객체 생성 및 함수 이름과 동일한 식별자에 할당까지 완료된 상태가 된다. 따라서 함수 선언문 이전에 함수를 참조 및 호출할 수 있다.

- 변수 호이스팅
  - 함수 표현식으로 함수를 정의하면 함수 호이스팅이 발생하는 것이 아니라 변수 호이스팅이 발생한다.
  - 따라서 `var` 키워드를 사용한 변수 선언문 이전에 변수를 참조하면 변수 호이스팅에 의해 `undefined`로 평가된다. 이때 함수를 호출하면 타입 에러(Type Error) 발생하므로, 반드시 함수 표현식 이후에 참조 또는 호출해야 한다.

### 12.4.4. Function 생성자 함수

자바스크립트가 기본 제공하는 빌트인 함수인 Function 생성자 함수에 매개변수 목록과 함수 몸체를 문자열로 전달하면서 new 연산자와 함께 호출하면 함수 객체를 생성해서 반환한다.

```js
var add = new Function('x', 'y', 'return x + y');

console.log(add(2, 5)); // 7
```

Function 생성자 함수로 생성한 함수는 클로저(closure)를 생성하지 않는 등, 함수 선언문이나 함수 표현식으로 생성한 함수와 다르게 동작한다.

### 12.4.5. 화살표 함수

ES6에 도입된 화살표 함수(arrow function)은 `function` 키워드 대신 화살표(fat arrow) `=>`를 사용해 좀 더 간략한 방법으로 함수를 선언한다. 화살표 함수는 항상 익명 함수로 정의한다.

- 화살표 함수는 생성자 함수로 사용할 수 없다.
- 기존 함수와 this 바인딩 방식이 다르다.
- prototype 프로퍼티가 없다.
- arguments 객체를 생성하지 않는다.

```js
// 화살표 함수
const add = (x, y) => x + y;
console.log(add(2, 5)); // 7
```

## 12.5. 함수 호출

함수를 가리키는 식별자와 한 쌍의 소괄호인 함수 호출 연산자(`()`)로 함수를 호출한다. 함수 호출 연산자 내에는 0개 이상의 인수를 쉼표로 구분해서 나열한다. 함수를 호출하면 현재의 실행 흐름을 중단하고 호출된 함수로 실행 흐름을 옮긴다. 이때 매개변수에 인수가 순서대로 할당되고 함수 몸체의 문들이 실행되기 시작한다.

### 12.5.1. 매개변수와 인수

함수를 실행하기 위해 필요한 값을 함수 외부에서 함수 내부로 전달할 때, 매개변수(parameter, 혹은 인자)를 통해 인수(argument)를 전달한다.
- 인수는 함수를 호출할 때 지정하며, 개수와 타입에 제한이 없다. 단, 값으로 평가될 수 있는 표현식이어야 한다.
- 매개변수는 함수를 정의할 때 선언하며, 함수 몸체 내부에서 변수와 동일하게 취급되어 undefined로 초기화된 이후 인수가 순서대로 할당된다. 함수 몸체 내부에서만 참조할 수 있는 매개변수의 스코프(유효 범위)는 함수 내부다. ([13. 스코프](./13_scope.md) 참고)

```js
// 함수 선언문
function add(x, y) {
  console.log(x, y); // 2 5
  return x + y;
}

// 함수 호출
// 인수 1과 2는 매개변수 x와 y에 순서대로 할당되고 함수 몸체의 문들이 실행된다.
var result = add(1, 2);

// add 함수의 매개변수 x, y는 함수 몸체 내부에서만 참조할 수 있다.
console.log(x, y); // ReferenceError: x is not defined
```

함수는 매개변수와 인수의 개수가 일치하는지 체크하지 않는다. 함수를 호출할 때 매개변수의 개수만큼 인수를 전달하지 않은 경우에도 에러가 발생하지 않는다.

```js
function add(x, y) {

  // arguments 객체는 가변 인자 함수를 구현할 때 유용하게 사용된다.
  console.log(arguments); // Arguments(3) [2, 5, 10, callee: ƒ, Symbol(Symbol.iterator): ƒ]

  return x + y;
}

// 인수가 부족해서 인수가 할당되지 않은 매개변수의 값은 undefined다.
console.log(add(2)); // NaN (2 + undefined)

// 매개변수보다 인수가 더 많은 경우 초과된 인수는 무시된다.
console.log(add(2, 5, 10)); // 7
```

### 12.5.2. 인수 확인

자바스크립트에서 함수를 정의할 때 적절한 인수가 전달되었는지 확인할 필요가 있다.
- 자바스크립트 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않기 때문에
- 자바스크립트는 동적 타입 언어로, 매개변수의 타입을 사전에 지정할 수 없기 때문에

1) 타입 체크
```js
function add(x, y) {
  if (typeof x !== 'number' || typeof y !== 'number') {
    // 매개변수를 통해 전달된 인수의 타입이 부적절한 경우 에러를 발생시킨다.
    throw new TypeError('인수는 모두 숫자 값이어야 합니다.');
  }

  return x + y;
}

console.log(add(2));        // TypeError: 인수는 모두 숫자 값이어야 합니다.
console.log(add('a', 'b')); // TypeError: 인수는 모두 숫자 값이어야 합니다.
```

2) 단축 평가 활용

```js
function add(a, b, c) {
  a = a || 0;
  b = b || 0;
  c = c || 0;
  return a + b + c;
}

console.log(add(1, 2, 3)); // 6
console.log(add(1, 2)); // 3
console.log(add(1)); // 1
console.log(add()); // 0
```

3) ES6 매개변수 기본값

```js
function add(a = 0, b = 0, c = 0) {
  return a + b + c;
}

console.log(add(1, 2, 3)); // 6
console.log(add(1, 2)); // 3
console.log(add(1)); // 1
console.log(add()); // 0
```

### 12.5.3. 매개변수의 최대 개수

- 이상적인 매개변수 개수는 0개이며 적을수록 좋다. 
- 매개변수는 최대 3개 이상을 넘지 않는 것을 권장한다.
- 그 이상의 매개변수가 필요하다면 하나의 매개변수를 선언하고 객체를 인수로 전달하는 것이 유리하다. 단, 함수 외부에서 내부로 전달한 객체를 함수 내부에서 변경하면 함수 외부의 객체가 변경되는 부수 효과(side effect)가 발생한다.

```js
$.ajax({
  method: 'POST',
  url: '/user',
  data: { id: 1, name: 'Lee' },
  cache: false
});
```

### 12.5.4. 반환문

`return` 키워드와 표현식(반환값)으로 이뤄진 반환문을 사용해 실행 결과를 함수 외부로 반환할 수 있다. 함수 호출 표현식은 표현식의 평가 결과, 즉 반환값으로 평가된다.

### 반환문의 역할

1. 함수의 실행을 중단하고 함수 몸체를 빠져나간다. (반환문 이후에 다른 문이 존재하면 그 문은 실행되지 않고 무시된다.)
2. `return` 키워드 뒤에 오는 표현식을 평가해 반환한다. (생략 및 명시적 지정 없다면 undefined 반환)

```js
function multiply(x, y) {
  // return 키워드와 반환값 사이에 줄바꿈이 있으면
  return // 세미콜론 자동 삽입 기능(ASI)에 의해 세미콜론이 추가된다.
  x * y; // 무시된다.
}

console.log(multiply(3, 5)); // undefined
```

반환문은 함수 몸체 내부에서만 사용할 수 있다. 전역에서 반환문을 사용하면 문법 에러(`SyntaxError: Illegal return statement`)가 발생한다.

```js
<!DOCTYPE html>
<html>
<body>
  <script>
    return; // SyntaxError: Illegal return statement
  </script>
</body>
</html>
```

- 참고. Node.js는 모듈 시스템에 의해 파일별로 독립적인 파일 스코프를 갖기 때문에, Node.js 환경에서는 파일의 가장 바깥 영역에 반환문을 사용해도 에러가 발생하지 않는다.

## 12.6. 참조에 의한 전달과 외부 상태의 변경

```js
// 매개변수 primitive는 원시 값을 전달받고, 매개변수 obj는 객체를 전달받는다.
function changeVal(primitive, obj) {
  primitive += 100;
  obj.name = 'Kim';
}

// 외부 상태
var num = 100;
var person = { name: 'Lee' };

console.log(num); // 100
console.log(person); // {name: "Lee"}

// 원시 값은 값 자체가 복사되어 전달되고 객체는 참조 값이 복사되어 전달된다.
changeVal(num, person);

// 원시 값은 원본이 훼손되지 않는다.
console.log(num); // 100

// 객체는 원본이 훼손된다.
console.log(person); // {name: "Kim"}
```

객체 타입 인수를 사용할 경우, 참조 값이 복사되어 매개변수에 전달되기 때문에 함수 외부에서 함수 몸체 내부로 전달한 참조 값에 의해 원본 객체가 변경되는 부수 효과가 발생한다.

#### 해결 방법

1) 객체를 불변 객체(immutable object)로 만들어 사용한다. 객체의 복사본을 새롭게 생성하는 비용은 들지만 객체를 마치 원시 값처럼 변경 불가능한 값으로 동작하게 만듬으로써 객체의 상태 변경을 원천봉쇄한다.
2) 객체의 상태 변경이 필요한 경우 객체의 방어적 복사(defensive copy)를 통해 원본 객체를 완전히 복제, 즉 깊은 복사(deep copy)를 통해 새로운 객체를 생성 및 재할당하여 교체한다.

## 12.7. 다양한 함수의 형태

### 12.7.1. 즉시 실행 함수

즉시 실행 함수(IFE, Immediately invoked Function Expression)는 함수 정의와 동시에 즉시 호출되는 함수이며, 단 한 번만 호출되며 다시 호출할 수 없다.

```js
// 익명 즉시 실행 함수
(function () {
  var a = 3;
  var b = 5;
  return a * b;
}());

// 기명 즉시 실행 함수
(function foo() {
  var a = 3;
  var b = 5;
  return a * b;
}());

foo(); // ReferenceError: foo is not defined

// 즉시 실행 함수도 일반 함수처럼 값을 반환할 수 있다.
var res = (function () {
  var a = 3;
  var b = 5;
  return a * b;
}());

console.log(res); // 15

// 즉시 실행 함수에도 일반 함수처럼 인수를 전달할 수 있다.
res = (function (a, b) {
  return a * b;
}(3, 5));

console.log(res); // 15
```

즉시 실행 함수는 반드시 그룹 연산자(`(...)`)로 감싸야 한다. 함수 리터럴을 평가해서 함수 객체를 생성하기 위해서다.

```js
function () { // SyntaxError: Function statements require a function name
  // ...
}();

function foo() {
  // ...
}(); // SyntaxError: Unexpected token ')'
```

즉시 실행 함수 내에 코드를 모아두면 혹시 있을 수도 있는 변수나 함수 이름의 충돌을 방지할 수 있다.

### 12.7.2. 재귀 함수

재귀 호출(recursive call)은 함수가 자기 자신을 호출하는 것을 의미한다. 재귀 함수(recursive function)은 재귀 호출을 수행하는 함수를 말한다.

```js
function countdown(n) {
  if (n < 0) return;
  console.log(n);
  countdown(n - 1); // 재귀 호출
}

countdown(10);
```

재귀 함수는 자신을 무한 재귀 호출하기 때문에, 재귀 함수 내에는 재귀 호출을 멈출 수 있는 탈출 조건을 반드시 만들어야 한다. 탈출 조건이 없으면 함수가 무한 호출되어 스택 오버플로우(stack overflow) 에러가 발생한다.

```js
// 팩토리얼(계승)은 1부터 자신까지의 모든 양의 정수의 곱이다.
// n! = 1 * 2 * ... * (n-1) * n
function factorial(n) {
  // 탈출 조건: n이 1 이하일 때 재귀 호출을 멈춘다.
  if (n <= 1) return 1;
  // 재귀 호출
  return n * factorial(n - 1);
}

console.log(factorial(0)); // 0! = 1
console.log(factorial(1)); // 1! = 1
console.log(factorial(2)); // 2! = 2 * 1 = 2
console.log(factorial(3)); // 3! = 3 * 2 * 1 = 6
console.log(factorial(4)); // 4! = 4 * 3 * 2 * 1 = 24
console.log(factorial(5)); // 5! = 5 * 4 * 3 * 2 * 1 = 120
```

재귀 함수는 반복문 없이 구현할 수 있다는 장점이 있지만 반복문을 사용하는 것보다 재귀 함수를 사용하는 편이 더 직관적으로 이해하기 쉬울 때만 한정적으로 사용하는 것이 바람직하다.

### 12.7.3. 중첩 함수

중첩 함수(nested function) 혹은 내부 함수(inner function)는 함수 내부에 정의된 함수를 말한다. 중첩 함수를 포함하는 함수는 외부 함수(outer function)이라 부르며, 중첩 함수는 외부 함수 내부에서만 호출할 수 있다. 일반적으로 중첩 함수는 자신을 포함하는 외부 함수를 돕는 헬퍼 함수(helper function)의 역할을 한다.

```js
function outer() {
  var x = 1;

  // 중첩 함수
  function inner() {
    var y = 2;
    // 외부 함수의 변수를 참조할 수 있다.
    console.log(x + y); // 3
  }

  inner();
}

outer();
```

함수 선언문의 경우 ES6 이전에는 코드의 최상위 또는 다른 함수 내부에서만 정의할 수 있었으나, ES6부터 함수 정의는 문이 위치할 수 있는 문맥이라면 어디든지 가능하다. 단, 호이스팅으로 인해 혼란이 발생할 수 있으므로 if 문이나 for 문 등의 코드 블록에서 함수 선언문을 통해 함수를 정의하는 것은 바람직하지 않다.

중첩 함수는 스코프, 클로저와 깊은 관련이 있다. [13. 스코프](./13_scope.md), [24. 클로저](./24_closure.md) 참고

### 12.7.4. 콜백 함수

콜백 함수(callback function)은 **함수의 매개 변수를 통해 다른 함수의 내부로 전달되는 함수**를 말하며, 매개 변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수를 고차 함수(Higher-Order Function, HOF)라고 한다. 콜백함수는 함수 외부에서 고차 함수 내부로 주입하기 때문에 자유롭게 교체할 수 있으며, 고차 함수에 전달되어 헬퍼 함수의 역할을 한다. 즉, 고차 함수는 콜백 함수를 자신의 일부분으로 합성한다.

고차 함수는 **매개 변수를 통해 전달받은 콜백 함수의 호출 시점을 결정해서 호출**한다. 즉, 콜백 함수는 고차 함수에 의해 호출되며, 이때 고차 함수는 필요에 따라 콜백 함수에 인수를 전달할 수 있다. 따라서 고차 함수에 콜백 함수를 전달할 때 함수를 호출하지 않고 함수 자체를 전달해야 한다.

콜백 함수가 고차 함수 내부에만 호출된다면 콜백 함수를 <ins>(1) 익명 함수 리터럴로 정의하면서 곧바로 고차 함수에 전달</ins>하는 것이 일반적이다.

```js
// 익명 함수 리터럴을 콜백 함수로 고차 함수에 전달한다.
// 익명 함수 리터럴은 repeat 함수를 호출할 때마다 평가되어 함수 객체를 생성한다.
repeat(5, function (i) {
  if (i % 2) console.log(i);
}); // 1 3
```

콜백 함수로서 전달된 함수 리터럴은 고차 함수가 호출될 때마다 평가되어 함수 객체를 생성한다.
콜백 함수를 다른 곳에서도 호출할 필요가 있거나, 콜백 함수를 전달 받는 함수가 자주 호출된다면 <ins>(2) 함수 외부에서 콜백 함수를 정의한 후 함수 참조를 고차 함수에 전달</ins>하는 편이 효율적이다.

```js
// logOdds 함수는 단 한 번만 생성된다.
var logOdds = function (i) {
  if (i % 2) console.log(i);
};

// 고차 함수에 함수 참조를 전달한다.
repeat(5, logOdds); // 1 3
```

콜백 함수는 함수형 프로그래밍 패러다임 뿐만 아니라 비동기 처리(이벤트 처리, Ajax 통신, 타이머 함수 등)에 활동되는 중요한 패턴이다.

```js
// 콜백 함수를 사용한 이벤트 처리
// myButton 버튼을 클릭하면 콜백 함수를 실행한다.
document.getElementById('myButton').addEventListener('click', function () {
  console.log('button clicked!');
});

// 콜백 함수를 사용한 비동기 처리
// 1초 후에 메시지를 출력한다.
setTimeout(function () {
  console.log('1초 경과');
}, 1000);
```

콜백 함수는 비동기 처리뿐 아니라 배열 고차 함수에서도 사용된다.

```js
// 콜백 함수를 사용하는 고차 함수 map
var res = [1, 2, 3].map(function (item) {
  return item * 2;
});

console.log(res); // [2, 4, 6]

// 콜백 함수를 사용하는 고차 함수 filter
res = [1, 2, 3].filter(function (item) {
  return item % 2;
});

console.log(res); // [1, 3]

// 콜백 함수를 사용하는 고차 함수 reduce
res = [1, 2, 3].reduce(function (acc, cur) {
  return acc + cur;
}, 0);

console.log(res); // 6
```

### 12.7.5. 순수 함수와 비순수 함수

순수 함수(pure function)는 함수형 프로그래밍에서 어떤 외부 상태에 의존하지도 않고 변경하지도 않는, 즉 부수 효과가 없는 함수를 뜻한다. 반대로 외부 상태에 의존하거나 외부 상태를 변경하는, 즉 부수 효과가 있는 함수를 비순수 함수(impure function)이라고 한다.

순수 함수는 동일한 인수가 전달되면 언제나 동일한 값을 반환한다. 어떤 외부 상태에도 의존하지 않고 오직 매개변수를 통해 함수 내부로 전달된 인수에게만 의존해 값을 생성해 반환하기 때문이다.
- 최소 하나 이상의 인수를 전달받는다.
- 순수 함수는 인수를 변경하지 않는다. (인수의 불변성을 유지한다.)
- 함수의 외부 상태를 변경하지 않는다.

```js
var count = 0; // 현재 카운트를 나타내는 상태

// 순수 함수 increase는 동일한 인수가 전달되면 언제나 동일한 값을 반환한다.
function increase(n) {
  return ++n;
}

// 순수 함수가 반환한 결과값을 변수에 재할당해서 상태를 변경
count = increase(count);
console.log(count); // 1

count = increase(count);
console.log(count); // 2
```

비순수 함수는 외부 상태(전역 변수, 서버 데이터, 파일, Console, DOM 등)에 따라 반환값이 달라지거나, 외부 상태에 의존하지 않아도 내부 상태가 호출될 때마다 변화하는 값(ex. 현재 시간) 을 반환한다. 즉, 외부 상태에 의존하거나 외부 상태를 변경하는 함수를 말한다.

```js
var count = 0; // 현재 카운트를 나타내는 상태: increase 함수에 의해 변화한다.

// 비순수 함수
function increase() {
  return ++count; // 외부 상태에 의존하며 외부 상태를 변경한다.
}

// 비순수 함수는 외부 상태(count)를 변경하므로 상태 변화를 추적하기 어려워진다.
increase();
console.log(count); // 1

increase();
console.log(count); // 2
```