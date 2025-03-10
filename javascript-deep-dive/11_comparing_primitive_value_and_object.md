# 11. 원시 값과 객체의 비교

- 원시 타입의 값, 즉 원시 값은 변경 불가능한 값(immutable value)이다. <br />이에 비해 객체(참조) 타입의 값, 즉 객체는 변경 가능한 값(mutable value)이다. 
- 원시 값을 변수에 할당하면 변수(확보된 메모리 공간)에는 실제 값이 저장된다. <br />이에 비해 객체를 변수에 할당하면 변수(확보된 메모리 공간)에는 참조 값이 저장된다.
- 원시 값을 갖는 변수를 다른 변수에 할당하면 원본의 **원시값이 복사되어 전달**된다. 이를 값에 의한 전달(pass by value)이라 한다. <br />이에 비해 객체를 가리키는 변수를 다른 변수에 할당하면 원본의 **참조 값이 복사되어 전달**된다. 이를 참조에 의한 전달(pass by reference)이라 한다.

## 11.1. 원시 값

### 11.1.1. 변경 불가능한 값

원시 타입의 값, 즉 원시 값은 변경 불가능한 값이다. 한번 생성된 원시 값은 읽기 전용 값으로서 변경할 수 없다.
- 값을 변경할 수 없다는 것은, 변수에 저장된 데이터로서 표현식이 평가되어 생성된 결과로서의 값이 변경 불가능하다는 뜻이다. 
- 즉, 원시 값 자체를 변경할 수 없다는 것이지 변수 값을 변경할 수 없다는 것이 아니다. 변수는 언제든지 재할당을 통해 변수 값의 변경(교체) 가능하기 때문이다.
- 이때, 상수와 변경 불가능한 값은 동일하지 않다. 상수는 재할당(값의 교체)가 금지된 변수일 뿐이다.

```js
// const 키워드를 사용해 선언한 변수는 재할당이 금지된다. 상수는 재할당이 금지된 변수일 뿐이다.
const o = {};

// const 키워드를 사용해 선언한 변수에 할당한 원시값(상수)은 변경할 수 없다.
// 하지만 const 키워드를 사용해 선언한 변수에 할당한 객체는 변경할 수 있다.
o.a = 1;
console.log(o); // {a: 1}
```

원시 값을 할당한 변수에 새로운 원시 값을 <ins>재할당</ins>하면 메모리 공간에 저장되어 있는 재할당 이전의 원시 값을 변경하는 것이 아니라, <ins>새로운 메모리 공간을 확보하고 재할당한 원시 값을 저장한 후 변수는 새롭게 재할당한 원시 값을 가리킨다. 이때, 변수가 참조하던 메모리 공간의 주소가 바뀐다.</ins>
- 변수가 참조하던 메모리 공간의 주소가 변경된 이유는 변수에 할당된 원시 값이 변경 불가능한 값이기 때문이다. 이러한 특성은 값의 **불변성**(immutability)이라 한다.
- 불변성을 갖는 원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경할 수 있는 방법이 없다. 

### 11.1.2. 문자열과 불변성

자바스크립트의 문자열은 원시 타입이며, 변경 불가능하다. 이것은 문자열이 생성된 이후에는 변경할 수 없음을 의미한다.

```js
var str = 'Hello';
str = 'world';
// 'Hello', 'world' 두 문자열 모두 메모리에 존재한다.
// 식별자 str은 문자열 'Hello'를 가리키고 있다가 문자열 'world'를 카리키도록 변경
```

문자열은 유사 배열 객체이면소 이터러블이므로 배열과 유사하게 각 문자에 접근할 수 있다.

```js
var str = 'string';

// 문자열은 유사 배열이므로 배열과 유사하게 인덱스를 사용해 각 문자에 접근할 수 있다.
// 하지만 문자열은 원시값이므로 변경할 수 없다. 이때 에러가 발생하지 않는다.
str[0] = 'S';

console.log(str); // string
```

문자열은 변경 불가능한 값이기 때문에 한번 생성된 문자열은 일기 전용 값으로서 변경할 수 없다. 원시 값은 어떤 일이 있어도 불변하기에, 예기치 못한 변경으로부터 자유로워 데이터의 신뢰성을 보장한다.

변수에 새로운 문자열을 재할당하는 것은 가능하다. 이는 기존 문자열을 변경하는 것이 아니라 새로운 문자열을 새롭게 할당하는 것이기 때문이다.

### 11.1.3. 값에 의한 전달

변수에 원시 값을 갖는 변수를 할당하면, 할당받는 변수에는 할당되는 변수의 원시 값이 복사되어 전달된다. 이를 값에 의한 전달(pass by value), 혹은 공유에 의한 전달(pass by sharing)이라 한다. 이때, **값에 의해 전달된 값은 다른 메모리 공간에 저장된 별개의 값**이다. 한쪽에서 재할당을 통해 값을 변경하더라도 서로 간섭할 수 없다.

엄격하게 표현하면 변수에는 값이 전달되는 것이 아니라 **메모리 주소**가 전달된다. 변수와 같은 식별자는 값이 아닌 메모리 주소를 기억하고 있기 때문이다. 전달된 메모리 주소를 통해 메모리 공간에 접근하면 값을 참조할 수 있다.

```js
var score = 80;

// copy 변수에는 score 변수의 값 80이 복사되어 할당된다.
var copy = score;
console.log(score, copy); // 80 80
console.log(score === copy); // true

// score 변수와 copy 변수의 값은 다른 메모리 공간에 저장된 별개의 값이다.
// 따라서 score 변수의 값을 변경해도 copy 변수의 값에는 어떠한 영향도 주지 않는다.
score = 100;
console.log(score, copy); // 100 80
```


## 11.2. 객체

객체는 프로퍼티의 개수가 정해져 있지 않으며, 동적으로 추가되고 삭제할 수 있다. 또한 프로퍼티의 값에도 제약이 없다. 따라서 객체는 원시 값과 같이 확보해야 할 메모리 공간의 크기를 사전에 정해 둘 수 없다. 원시 값은 상대적으로 적은 메모리를 소비하지만 객체는 경우에 따라 크기가 매우 클 수도 있다. 객체를 생성하고 프로퍼티에 접근하는 것도 원시 값과 비교할 때 비용이 많이 드는 일이다. 따라서 객체는 원시 값과는 다른 방식으로 동작하도록 설계되어 있다.

#### 자바스크립트 객체의 관리 방식

자바스크립트 객체는 프로퍼티 키를 인덱스로 사용하는 해시 테이블(hash table, 연관 배열(associative array), map, dictionary, lookup table이라고도 부르기도 한다.)이라고 생각할 수 있다.

자바스크립트는 클래스 없이 객체를 생성할 수 있으며, 객체가 생성된 이후라도 동적으로 프로퍼티와 메서드를 추가할 수 있다. 이는 이론적으로 클래스 기반 객체지향 프로그래밍 언어의 객체보다 성능적으로 생성과 프로퍼티 접근에 비용이 더 많이 드는 비효율적인 방식이다.

따라서 V8 자바스크립트 엔진은 프로퍼티에 접근하기 위해 동적 탐색(dynamic lookup) 대신 히든 클래스(hidden class)라는 방식을 사용해 C++ 객체의 프로퍼티에 접근하는 정도의 성능을 보장한다. 히든 클래스는 자바와 같이 고정된 객체 레이아웃(클래스)와 유사하게 동작한다.

### 11.2.1. 변경 가능한 값

객체(참조) 타입의 값, 즉 객체는 변경 가능한 값(mutable value)이다.

원시 값을 할당한 변수는 메모리 주소를 통해 메모리 공간에 접근하면 원시 값에 접근할 수 있다. 즉, 원시 값을 할당한 변수는 <ins>원시 값 자체</ins>를 값으로 갖지만, <br />
객체를 할당한 변수가 기억하는 메모리 주소를 통해 메모리 공간에 접근하면 **참조 값**(reference value)에 접근할 수 있다. 참조 값은 <ins>생성된 객체가 저장된 메모리 공간의 주소</ins>라 할 수 있다.

객체를 할당한 변수를 참조하면 메모리에 저장되어 있는 참조 값을 통해 실제 객체에 접근한다. "변수는 객체를 참조하고 있다" 또는 "변수는 객체를 가리키고(point) 있다"라고 표현한다.

```js
// 할당이 이뤄지는 시점에 객체 리터럴이 해석되고, 그 결과 객체가 생성된다.
var person = {
  name: 'Lee'
};

// person 변수에 저장되어 있는 참조값으로 실제 객체에 접근해서 그 객체를 반환한다.
console.log(person); // {name: "Lee"}
```

원시 값과는 다르게 객체는 변경 가능한 값이기 때문에 객체를 할당한 변수는 **재할당 없이 객체를 직접 변경**할 수 있다. 즉, 재할당 없이 프로퍼티의 동적 추가, 프로퍼티 값의 갱신, 프로퍼티 자체의 삭제가 가능하다.

원시 값을 갖는 변수의 값을 변경하려면 재할당을 통해 메모리에 원시 값을 새롭게 생성해야 하지만, 객체는 변경 가능한 값이므로 메모리에 저장된 객체를 직접 수정할 수 있다는 것이다. 이때 객체를 할당한 변수에 <ins>재할당 하지 않았으므로 객체를 할당한 변수의 참조 값은 변경되지 않는다.</ins>

```js
var person = {
  name: 'Lee'
};

// 프로퍼티 값 갱신
person.name = 'Kim';

// 프로퍼티 동적 생성
person.address = 'Seoul';

console.log(person); // {name: "Kim", address: "Seoul"}
```

객체를 생성하고 관리하는 방식은 매우 복잡하며 비용이 많이 드는 일이기 때문에 메모리의 효율적 소비가 어렵고 성능이 나빠진다.

메모리를 효율적으로 사용하기 위해, 그리고 객체를 복사해 생성하는 비용을 절약하여 성능을 향상시키기 위해, 객체는 변경 가능한 값으로 설계되어 있는 것이다.

객체는 이러한 구조적 단점에 따른 부작용이 있다. 원시 값과는 다르게 **여러 개의 식별자가 하나의 객체를 공유할 수 있다**는 것이다.

#### 얕은 복사와 깊은 복사

- 얕은 복사 (shallow copy): 객체를 프로퍼티 값으로 갖는 객체의 경우 한 단계까지만 복사하는 것. 객체를 할당한 변수를 다른 변수에 할당하는 것.
- 깊은 복사 (deep copy): 객체를 프로퍼티 값으로 갖는 객체의 경우 객체에 중첩되어 있는 객체까지 모두 복사하는 것. 원시 값을 할당한 변수를 다른 변수에 할당하는 것.

모두 원본과는 다른 객체(참조 값이 다른 별개의 객체)이지만, 얕은 복사는 객체에 중첩되어 있는 객체의 경우 참조값을 복사하고, 깊은 복사는 객체에 중첩되어 있는 객체까지 모두 복사해서 원시 값처럼 완전한 복사본을 만든다.

```js
const o = { x: { y: 1 } };

// 얕은 복사
const c1 = { ...o }; // 35장 "스프레드 문법" 참고
console.log(c1 === o); // false
console.log(c1.x === o.x); // true

// lodash의 cloneDeep을 사용한 깊은 복사
// "npm install lodash"로 lodash를 설치한 후, Node.js 환경에서 실행
const _ = require('lodash');
// 깊은 복사
const c2 = _.cloneDeep(o);
console.log(c2 === o); // false
console.log(c2.x === o.x); // false
```

### 11.2.2. 참조에 의한 전달

객체는 여러 개의 식별자가 하나의 객체를 공유할 수 있으며, 이로 인해 부작용이 발생할 수 있다.

객체를 가리키는 변수를 다른 변수에 할당하면 원본의 **참조 값이 복사되어 전달**된다. 이를 참조에 의한 전달이라 한다. 결국 저장된 메모리 주소는 다르지만 동일한 참조 값을 갖게 되어 모두 동일한 객체를 가리켜 두 개의 식별자가 하나의 객체를 공유하게 된다. 따라서 원본 또는 사본 중 어느 한쪽에서 객체를 변경(변수에 새로운 객체를 재할당하는 것이 아닌, 객체의 프로퍼티 값 변경이나 프로퍼티 추가/삭제)하면 서로 영향을 주고 받는다.

```js
var person = {
  name: 'Lee'
};

// 참조값을 복사(얕은 복사). copy와 person은 동일한 참조값을 갖는다.
var copy = person;

// copy와 person은 동일한 객체를 참조한다.
console.log(copy === person); // true

// copy를 통해 객체를 변경한다.
copy.name = 'Kim';

// person을 통해 객체를 변경한다.
person.address = 'Seoul';

// copy와 person은 동일한 객체를 가리킨다.
// 따라서 어느 한쪽에서 객체를 변경하면 서로 영향을 주고 받는다.
console.log(person); // {name: "Kim", address: "Seoul"}
console.log(copy);   // {name: "Kim", address: "Seoul"}
```

결국 "참조에 의한 전달"은 **식별자가 기억하는 메모리 공간에 저장되어 있는 값을 복사해서 전달한다**는 면에서 "값에 의한 전달"과 동일하지만, <br />
식별자가 기억하는 메모리 공간, 즉 변수에 저장되어 있는 값이 원시 값이 아닌 **참조 값**이라는 차이점이 있다.

ESMAScript 사양에 정의된 자바스크립트의 공식적인 용어는 아니며 정확한 동작방식을 설명하지 못한다. 또한, "값에 의한 전달"과 "참조에 의한 전달" 모두 "**공유에 의한 전달**"이라고 표현하는 경우도 있다. 전달되는 값의 종류가 원시 값인지 참조 값인지 구별해서 강조하는 의미로 사용되는 용어로서 기억하자.

```js
var person1 = {
  name: 'Lee'
};

var person2 = {
  name: 'Lee'
};

// 두 변수가 가리키는 객체는 내용은 같지만 다른 메모리에 저장된 별개의 객체이므로,
// 두 변수의 참조 값은 전혀 다른 값이다.
console.log(person1 === person2); // false

// 프로퍼티 값을 참조하는 person1.name, person2.name은 값으로 평가될 수 있는 표현식이며
// 두 표현식 모두 원시 값 'Lee'로 평가된다.
console.log(person1.name === person2.name); // true
```
