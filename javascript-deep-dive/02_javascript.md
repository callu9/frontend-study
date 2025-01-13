# 2. 자바스크립트

## 2.2. 자바스크립트의 표준화

1996년 8월, 마이크로소프트는 자바스크립트의 파생 버전인 “JScript”를 인터넷 익스플로러 3.0에 탑재했고, 자바스크립트가 표준화되지 못하고 적당히 호환되었다는 문제가 발생했다. 즉, 넷스케이프 커뮤니케이션즈와 마이크로소프트는 자사 브라우저의 시장 점유율을 높이기 위해 자사 브라우저에서만 동작하는 기능을 경쟁적으로 추가하기 시작했다는 것.

이로 인해 브라우저에 따라 웹페이지가 정상적으로 동작하지 않는 **크로스 브라우징 이슈**가 발생하기 시작했고, 결과적으로 모든 브라우저에서 정상적으로 동작하는 웹페이지를 개발하기 위하여 자바스크립트의 파편화를 방지하고 모든 브라우저에서 정상적으로 동작하는 표준화된 자바스크립트의 필요성이 대두되기 시작했다. 이를 위해 1996년 11월, 넷스케이프 커뮤니케이션즈는 컴퓨터 시스템의 표준을 관리하는 비영리 표준화 기구인 ECMA 인터내셔널에 자바스크립트의 표준화를 요청한다.

| 버전                   | 출시 연도 | 특징                                                                                                                                                                                                                                                    |
| ---------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ES1                    | 1997      | 초판                                                                                                                                                                                                                                                    |
| ES2                    | 1998      | IS0/IEC 16262 국제 표준과 동일한 규격을 적용                                                                                                                                                                                                            |
| ES3                    | 1999      | 정규표현식, `try ... catch`                                                                                                                                                                                                                             |
| ES5                    | 2009      | HTML5와 함께 출현한 표준안.<br />JSON, strict mode, 접근자 프로퍼티, 프로퍼티 어트리뷰트 제어, 향상된 배열 조작 기능(`forEach`, `map`, `filter`, `reduce`, `some`, `every`)                                                                             |
| ES6 (ECMAScript 2015)  | 2015      | `let/const`, 클래스, 화살표 함수, 템플릿 리터럴, 디스트럭처링 할당, 스프레드 문법, `rest` 파라미터, 심벌, 프로미스, Map/Set, 이터러블, `for ... of`, 제너레이터, Proxy, 모듈 import/export                                                              |
| ES7 (ECMAScript 2016)  | 2016      | 지수(`**`) 연산자, `Array.prototype.includes`, `String.prototype.includes`                                                                                                                                                                              |
| ES8 (ECMAScript 2017)  | 2017      | async/await, Object 정적 메서드(`Object.values`, `Object.entries`, `Object.getOwnPropertyDescriptors`)                                                                                                                                                  |
| ES9 (ECMAScript 2018)  | 2018      | Object rest/spread 프로퍼티, `Promise.prototype.finally`, async generator, `for await ... of`                                                                                                                                                           |
| ES10 (ECMAScript 2019) | 2019      | `Object.fromEntries`, `Array.prototype.flat`, `Array.prototype.flatMap`, optional catch binding                                                                                                                                                         |
| ES11 (ECMAScript 2020) | 2020      | `String.prototype.matchAll`, `BigInt`, `globalThis`, `Promise.allSettled`, null 병합 연산자, 옵셔널 체이닝 연산자, `for ... in enumeration order`                                                                                                       |
| ES12 (ECMAScript 2021) | 2021      | `String.prototype.replaceAll`, `Promise.any`, AggregateError, 논리 할당 연산자 (logical assignment operators, `??=`, `&&=`, `\|\|=`), WeakRef, Numeric separators(`1_000`), `Array.prototype.sort`                                                      |
| ES13 (ECMAScript 2022) | 2022      | top-level await, private class fields(`#`), class `static` keyword, the `#x` in obj syntax, <ins>RegExp `d` 플래그</ins>, `Error` 인스턴스 `cause` 데이터 속성, `String.prototype.at`, `Array.prototype.at`, `TypedArray.prototype.at`, `Object.hasOwn` |
| ES14 (ECMAScript 2023) | 2023      | 배열 조작 기능 (`toSorted`, `toReversed`, `with`, `findLast`, `findLastIndex`, `toSpliced`), <ins>해시뱅(Hashbang)</ins>, `WeakMap`의 키로 `Symbol` 사용                                                                                                |
| ES15 (ECMAScript 2024) | 2024      | ArrayBuffer의 `resize`와 `transfer` 메소드, <ins>RegExp `v` 플래그</ins>, `Promise.withResolvers`, `Object.groupBy`, `Map.groupBy`, `Atomics.waitAsync`, String의 `isWellFormed`와 `toWellFormed` 메소드                                                |

### 참고

- Regex `d` 플래그: 일치하는 문자의 인덱스 배열(`[start, end]`) 생성<br/>

```js
const str1 = "foo bar foo";
const regex1 = /foo/d;
console.log(regex1.hasIndices); // true
console.log(regex1.exec(str1).indices[0]); // [0, 3]
console.log(regex1.exec(str1).indices[0]); // [8, 11]
```

- 해시뱅(Hashbang) 혹은 셔뱅(Shebang) 문법: 주로 UNIX 계열 OS에서 사용하는<br />
  스크립트 상단에 작성하는 실행될 프로그램의 인터프리터를 정의하는 코드. <br />자바스크립트 파일의 첫 라인이 `#!`로 시작될 경우, 해당 라인을 무시.

```js
#! /usr/bin/env node
const str = "Hello World";
console.log(str);
```

- Regex `u` 플래그: `pattern`을 유니코드 코드 포인트의 시퀀스로 처리
- Regex `v` 플래그: 문자열 속성뿐만 아니라 문자 클래스의 집합 표기법을 활성화하는 u 플래그의 업그레이드

```js
const regex = /\p{L}/v; // 모든 언어의 문자만 매칭
const str = "foo bar 123!";
const matches = str.match(regex);
console.log(matches); // ['f', 'o', 'o', 'b', 'a', 'r']
```

## 2.3. 자바스크립트 성장의 역사

초창기 자바스크립트는 웹페이지의 보조적인 기능을 수행하기 위해 한정적인 용도로 사용되었다.<br />
이 시기에 대부분의 로직은 주로 웹 서버에서 실행되었고, 브라우저는 서버로부터 전달받은 HTML과 CSS를 단순히 *렌더링*하는 수준이었다.

- 렌더링(rendering): HTML, CSS, 자바스크립트로 작성된 문서를 해석해서 브라우저에 시각적으로 출력하는 것

### 2.3.1. Ajax

#### 1) Ajax의 등장

1999년, XHMLHttpRequest라는 이름으로 자바스크립트를 이용해 **서버와 브라우저가 비동기\(asynchronous\) 방식으로 데이터를 교환**할 수 있는 통신 기능인 Ajax(Asynchronous Javascript and XML)가 등장했다.

#### 2) Ajax의 장점

이전의 웹페이지와 같이 웹페이지에서 변경할 필요가 없는 부분을 렌더링 하지 않고, 서버로부터 필요한 데이터만 전송받아 변경해야 하는 부분만 한정적으로 렌더링하는 방식이 가능해졌다.<br />
즉, 웹 브라우저에서도 데스크톱 애플리케이션과 유사한 <ins>빠른 성능과 부드러운 화면 전환이 가능</ins>해졌다. (처음부터 다시 렌더링하여 화면이 전환되면 순간적으로 화면 깜빡임 현상이 발생하는 한계가 사라졌다.)

#### 3) Ajax 사용 사례

2005년, 구글 맵스(Google Maps)는 웹 브라우저에서 자바스크립트와 Ajax를 기반으로 동작하여, 데스크톱 애플리케이션과 비교했을 때 손색이 없을 정도의 성능과 부드러운 화면 전환 효과를 보여주었다.

### 2.3.2. jQuery

#### 1) jQuery의 등장

2006년, jQuery의 등장으로 다소 번거롭고 논란이 있던 DOM(Document Object Model)을 더욱 쉽게 제어할 수 있게 되었고 크로스 브라우징 이슈도 어느정도 해결되었다.

#### 2) jQuery의 장점

자바스크립트보다 배우기 쉽고 직관적이며, DOM 제어가 쉽다는 장점이 있다.

### 2.3.3. V8 자바스크립트 엔진

#### 1) V8 자바스크립트 엔진의 등장

2008년, 구글의 자바스크립트 엔진은 자바스크립트로 웹 애플리케이션을 구축하려는 시도가 늘면서 더욱 빠르게 동작하는 자바스크립트 엔진의 필요성에 대한 요구를 충족하기 위해 등장하였다.<br />
V8 자바스크립트 엔진의 등장으로 자바스크립트는 데스크톱 애플리케이션과 유사한 사용자 경험(UX, User Experience)을 제공할 수 있는 웹 애플리케이션 프로그래밍 언어로 정착하게 되었다.

#### 2) V8 자바스크립트 엔진의 장점

빠른 성능 및 과거 웹 서버에서 수행되던 로직을 클라이언트(브라우저)로 이동

### 2.3.4. Node.js

#### 1) Node.js의 등장

2009년, 라이언 달(Ryan Dhal)이 구글 V8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임 환경(runtime environment)인 Node.js를 발표하였다.<br />
Node.js는 자바스크립트를 브라우저 이외의 환경에서도 동작할 수 있도록 자바스크립트 엔진을 브라우저에서 독립시킨 자바스크립트 실행 환경이다.<br />
주로 서버 사이드 애플리케이션 개발에 주로 사용되며, 이에 필요한 모듈, 파일 시스템, HTTP 등 빌트인 API를 제공한다.<br />
자바스크립트는 **크로스 플랫폼**을 위한 가장 중요한 언어로 주목받게 되었다.

#### 2) Node.js의 장점

- 서버 사이드 어플리케이션 개발 가능: 자바스크립트 엔진을 기반으로 하므로, 프론트엔드와 백엔드 영역에서 자바스크립트를 사용할 수 있다는 동형성(isomorphic)으로 인해 별도의 언어 학습 시간 감축 가능.
- 요청 처리 성능이 좋음: 비동기 I/O를 지원하며, 단일 스레드 이벤트 루프 기반으로 동작하기 때문이다. Node.js는 데이터를 실시간으로 처리하기 위해 I/O가 빈번하게 발생하는 SPA에 적합하다. <br/>(하지만 CPU 사용률이 높은 애플리케이션에는 권장하지 않음.)

### 2.3.5. SPA 프레임워크

#### 1) SPA 프레임워크의 등장


개발 규모와 복잡도가 상승함으로써, 변경에 유연하면서 확장하기 쉬운 애플리케이션 아키텍처 구축을 위해 CBD(Component based development) 방법론을 기반으로 SPA(Single Page Application)가 대중화되었다.<br /> Angular, React, Vue.js, Svelte 등 다양한 SPA 프레임워크/라이브러리 또한 많은 사용층을 확보하게 되었다.


## 2.4. 자바스크립트와 ECMAScript

### ECMAScript

자바스크립트의 표준 사양인 ECMA-262를 말하며, <br/>프로그래밍 언어의 값, 타입, 객체와 프로퍼티, 함수, 표준 빌트인 객체(standard built-in object) 등 핵심 문법을 규정한다.

자바스크립트란, 기본 뼈대인 ECMAScript와 브라우저가 별도 지원하는 **클라이언트 사이드 Web API**를 아우르는 개념.
- 클라이언트 사이드 Web API
  - DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker... 등
  - ECMAScript와는 별도로 월드 와이드 웹 콘소시엄(World Wide Consortium:W3C)에서 별도의 사양으로 관리
  - 자세한 내용은 [MDN web docs : Web API](https://developer.mozilla.org/ko/docs/Web/API) 페이지 참고

## 2.5. 자바스크립트의 특징

## 2.6. ES6 브라우저 지원 현황

https://caniuse.com/ 참고
