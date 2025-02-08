# 1. var, let, const 비교 (중복 선언 허용, 스코프, 호이스팅)

| 특성                 | var                   | let             | const           |
| ------------------ | --------------------- | --------------- | --------------- |
| 중복 선언 허용           | O                     | X               | X               |
| 스코프(Scope)<br>     | 함수 스코프                | 블록 스코프          | 블록 스코프          |
| 호이스팅(Hoisting)<br> | 선언+초기화<br>(undefined) | 선언만 <br>(초기화 X) | 선언만 <br>(초기화 X) |
| 재할당 가능 여부          | O                     | O               | X               |

### 중복 선언 허용
- var은 동일한 변수명을 여러 번 선언할 수 있지만, **let과 const는 중복 선언이 불가능**하다.
```js
var a = 1;
var a = 2; // 가능

let b = 1;
let b = 2; // 에러 발생 (SyntaxError)

const c = 1;
const c = 2; // 에러 발생 (SyntaxError)
```

### 스코프(Scope)
- var은 함수 스코프를 가지며, 함수 안에서 선언되면 외부에서 접근할 수 없다.
- let과 const는 블록 스코프를 가지며, {} 안에서만 유효하다.
```js
function test() {
    if (true) {
        var x = 10;
        let y = 20;
    }
    console.log(x); // 10 (함수 스코프)
    console.log(y); // 에러 (블록 스코프)
}
```

### 호이스팅(Hoisting)
- var은 선언과 동시에 undefined로 초기화됩니다.
- let과 const는 선언만 되고 초기화되지 않아서 접근하면 에러가 발생합니다.
```js
console.log(a); // undefined
var a = 10;

console.log(b); // 에러 (Cannot access 'b' before initialization)
let b = 20;
```

# 2. 브라우저는 어떻게 동작하는가

### 1. 브라우저의 핵심 구성 요소
- 사용자 인터페이스 (UI): 주소창, 뒤로 가기, 새로고침 등의 요소
- 렌더링 엔진: HTML, CSS를 화면에 표시하는 역할 (ex. 크롬의 Blink)
- 자바스크립트 엔진: 자바스크립트 실행 (ex. Chrome V8)
- 네트워크 모듈: HTTP 요청 및 응답 처리
- 데이터 저장소: 쿠키, 로컬스토리지, 세션스토리지 등
### 2. 브라우저의 동작 과정
1. URL 입력 → 사용자가 주소창에 https://example.com 입력
2. DNS 조회 → example.com의 실제 IP 주소를 찾음
3. 서버 요청 → IP 주소로 해당 웹사이트의 HTML을 요청
4. 응답 받기 → 서버가 HTML, CSS, JavaScript 등을 응답으로 보냄
5. 렌더링: HTML 파싱 → DOM 생성
	- CSS 파싱 → 스타일을 적용하여 렌더 트리(Render Tree) 생성
	- 자바스크립트 실행 → 페이지 동적 조작
	- 레이아웃 계산 & 페인트(Paint) → 최종 화면 출력

# 정리
- var, let, const → 선언 방식의 차이 (스코프, 중복 선언, 호이스팅)
- 브라우저 동작 원리 → HTML → CSS → JS → 렌더링 과정
