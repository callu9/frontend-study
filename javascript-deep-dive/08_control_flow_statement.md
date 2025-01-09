# 8장. 제어문

제어문(control flow statement)은 조건에 따라 코드 블록을 실행(조건문)하거나 반복 실행(반복문)할 때 사용한다. 일반적으로 코드는 위에서 아래 방향으로 순차적으로 실행되는데, 제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할수 있다.<br />
하지만 코드의 실행 순서가 변경된다는 것은 단순히 위에서 아래로 순차적으로 진행하는 직관적인 코드의 흐름을 혼란스럽게 만든다. 따라서 제어문은 코드의 흐름을 이해하기 어렵게 만들어 가독성을 해치는 단점이 있다. 가독성이 좋지 않은 코드는 오류를 발생시키는 원인이 된다.

- (참고) forEach，map, filter, reduce 같은 고차 함수를 사용한 함수형 프로그래밍 기법에서는 제어문의 사용을 억제하여 복잡성을 해결하려고 노력한다.

## 8.1. 블록문

블록문(block statement/compound statement)은 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부르기도 한다. 자바스크립트는 블록문을 하나의 실행 단위로 취급한다. 블록문은 단독으로 사용할 수도 있으나 일반적으로 제어문이나 함수를 정의할 때 사용하는 것이 일반적이다.

주의: 블록문은 언제나 문의 종료를 의미하는 자체 종결성을 갖기 때문에 블록문의 끝에는 세미콜론을 붙이지 않는다.

```js
// 블록문
{
  var foo = 10;
}

// 제어문
var x = 1;
if (x < 10) {
  x++;
}

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```

## 8.2. 조건문

조건문(conditional statement)은 주어진 조건식(conditional expression)의 평가 결과에 따라 코드 블록(블록문)의 실행을 결정한다.

- 조건식: boolean 값으로 평가될 수 있는 표현식

### 8.2.1. if ... else 문

`if ... else` 문은 주어진 조건식(불리언 값으로 평가될 수 있는 표현식)의 평가 결과, 즉논리적 참 또는 거짓
에 따라 실행할 코드 블록을 결정한다. 조건식의 평가 결과가 true일 경우 `if` 문의 코드 블록이 실행되고,
false일 경우 `else` 문의 코드 블록이 실행된다.

```js
if (조건식1) {
  // 조건식1이 참이면 이 코드 블록이 실행된다.
} else if (조건식2) {
  // 조건식2이 참이면 이 코드 블록이 실행된다.
} else {
  // 조건식1과 조건식2가 모두 거짓이면 이 코드 블록이 실행된다.
}
```

만약 `if` 문의 조건식이 불리언 값이 아닌 값으로 평가되면, 자바스크립트 엔진에 의해 암묵적으로 불리언 값으로 강제 변환되어 실행할 코드 블록을 결정한다.
([9.2. 암묵적 타입 변환](./09_type_conversion_and_short_circuit_evaluation.md#92-암묵적-타입-변환) 참고)

대부분의 `if ... else` 문은 **삼항 조건 연산자**로 바꿔 쓸 수 있다.

- (참고) 삼항 조건 연산자는 값으로 평가되는 <ins>표현식</ins>을 만든다. 따라서 삼항 조건 연산자 표현식은 값처럼 사용할 수 있기 때문에 <ins>변수에 할당할 수 있다.</ins>

```js
var num = 2;

// 0은 false로 취급된다.
var kind = num ? (num > 0 ? "양수" : "음수") : "영";
console.log(kind); // 양수
```

### 8.2.2. switch 문

`switch` 문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 `case` 문으로 실행 흐름을 옮긴다.<br/>
`case` 문은 상황(case)을 의미하는 표현식을 지정하고 콜론으로 마친다. 그리고 그 뒤에 실행할 문들을 위치시킨다. `switch` 문의 표현식과 일치하는 `case` 문이 없다면 실행 순서는 `default` 문으로 이동한다. `default` 문은 선택사항으로, 사용할 수도 있고 사용하지 않을 수도 있다.

```js
switch (표현식) {
  case 표현식1:
    // switch 문의 표현식과 표현식1이 일치하면 실행될 문;
    break;
  case 표현식2:
    // switch 문의 표현식과 표현식2가 일치하면 실행될 문;
    break;
  default:
  // switch 문의 표현식과 일치하는 case 문이 없을 때 실행될 문;
}
```

- (참고) `break;`를 사용하지 않고 `switch` 문이 끝날 때까지 이후의 모든 `case` 문과 `default` 문을 실행한 경우를 폴스루(fall through)라고 한다.

- `if ... else` 문은 논리적 참, 거짓으로 실행할 코드 블록을 결정한다.
- `switch` 문은 다양한 상황(case)에 따라 실행할 코드 블록을 결정할 때 사용한다. <br/>(불리언 값보다는 문자열이나 숫자 값인 경우가 많다)

## 8.3. 반복문

반복문(loop statement)은 조건식의 평가 결과가 참인 경우 코드 블록을 실행한다. 그 후 조건식을 다시 평가하여 여전히 참인 경우 코드 블록을 다시 실행하고, 이는 조건식이 거짓일 때까지 반복된다.

### 8.3.1. for 문

`for` 문의 형태

```js
for (변수 선언문 또는 할당문; 조건식; 증감식) {
  // 조건식이 참인 경우 반복 실행될 문;
}
```

일반적인 예제

```js
for (var i = 0; i < 2; i++) {
  console.log(i);
}
// 0
// 1
```

어떤 식도 선언하지 않으면 무한루프가 된다.

```js
//무한루프
for (;;) { ... }
```

중첩 `for` 문

```js
for (var i = 1; i <= 6; i++) {
  for (var j = 1; j <= 6; j++) {
    if (i + j === 6) console.log(`[${i}, ${j}]`);
  }
}
```

### 8.3.2. while 문

`while` 문은 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행한다. <br/>`for` 문은 반복 횟수가 명확할 때 주로 사용하고, `while` 문은 반복 횟수가 불명확할 때 주로 사용한다.

일반적인 예제

```js
var count = 0;
// 무한루프
while (true) {
  console.log(count);
  count++;
  // count가 3이면 코드 블록을 탈출한다.
  if (count === 3) break;
} // 0 1 2
```

조건식의 평가 결과가 언제나 참이면 무한루프가 된다.

```js
//무한루프
for (;;) { ... }
```

### 8.3.3. do ... while 문

`do ... while` 문은 코드 블록을 먼저 실행하고 조건식을 평가한다. 따라서 코드 블록은 무조건 한 번 이상 실행된다.

```js
var count = 0;
// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
do {
  console.log(count); // 0 1 2
  count++;
} while (count < 3);
```

## 8.4. break 문

`break` 문은 레이블 문, 반복문(`for`, `for ... in`, `for ... of`, `while`, `do ... while`) 또는 `switch` 문의 코드 블록을 탈출한다. <br />레이블 문, 반복문, `switch` 문의 코드 블록 외에 `break` 문을 사용하면 SyntaxError(문법 에러)가 발생한다.

```js
if (true) {
  break; // Uncaught SyntaxError: Illegal break statement
}

// foo라는 식별자가 붙은 레이블 블록문
foo: {
  console.log(1);
  break foo; // foo 레이블 블록문을 탈출한다.
  console.log(2);
}
console.log('Done!');

// outer라는 식별자가 붙은 레이블 for 문
outer: for (var i = 0; i < 3; i++) {
  for (var j = 0; j < 3; j++) {
    // i + j === 3이면 outer라는 식별자가 붙은 레이블 for 문을 탈출한다.
    if (i + j === 3) break outer;
    console.log (`inner [${ i }, ${j }]`) ;
  }
}
console.log('Done!');
```

## 8.5. continue 문

`continue` 문은 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다. `break` 문처럼 반복문을 탈출하지는 않는다.

```js
var string = "Hello World.";
var search = "l";
var count = 0;

// 문자열은 유사 배열이므로 for 문으로 순회할 수 있다.
for (var i = 0; i < string.length; i++) {
  // 'l'이 아니면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.
  if (string[i] !== search) continue;
  count++; // continue 문이 실행되면 이 문은 실행되지 않는다.
}
console.log(count); // 3
```
