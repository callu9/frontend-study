# useCallback

- React Hook으로, **함수를 메모이제이션**하여 **불필요한 함수 재생성을 방지**하는 역할

```js
const memoizedCallback = useCallback(
  () => {
    console.log("Hello, World!");
  },
  [] // 의존성 배열
);
// 빈 배열을 의존성으로 가지므로, 컴포넌트가 리렌더링 되어도 함수가 재생성되지 않음.
```

## 모던 자바스크립트 특성과의 연관성

### 1. 함수는 1급 객체 (First-Class Citizen)

자바스크립트에서 함수는 1급 객체이다. 따라서

- 변수에 할당할 수 있다.
- 다른 함수의 인자로 전달될 수 있다.
- 반환값으로도 사용할 수 있다.

```js
const func = () => console.log("Hello!");
const anotherFunc = func; // 함수 대입 가능
anotherFunc(); // Hello!
```

하지만 리액트에서는 **리렌더링 시마다 새로운 함수가 재생성**되므로 성능 문제가 발생할 수 있다.

```js
const handleClick = () => console.log("Clicked!");

// useEffect에 handleClick을 넣었을 때
useEffect(() => {
  console.log("Effect runs!");
}, [handleClick]); // handleClick이 매번 새로 생성되어 useEffect가 계속 실행됨!
```

➡ useCallback을 사용하면 **기존 함수 참조값을 유지**하여, 위와 같은 **불필요한 재실행을 방지**할 수 있다.

### 2. 클로저 (Closure)

클로저란, **내부 함수가 외부 함수의 스코프에 접근할 수 있는 기능**

```js
function outer() {
  var count = 0;
  return function inner() {
    count++;
    console.log(count);
  };
}

const increment = outer();
increment(); // 1
increment(); // 2
```

➡ useCallback은 클로저를 활용하여 **이전 상태의 변수를 유지**하면서 **참조값을 변경하지 않는다**.

```js
const handleClick = useCallback(() => {
  console.log("Clicked!");
}, []); // 빈 배열이므로 handleClick의 참조값이 변경되지 않음
```

- 클로저 덕분에 useCallback 내부에서 이전 상태를 기억하고 있음
- 의존성 배열이 변경되지 않으면, 이전에 생성된 함수를 재사용함

### 3. 참조형 데이터 (Reference Type)

자바스크립트의 객체, 배열, 함수 등은 참조형 데이터이므로, 메모리 주소(**참조값**)을 비교하여 변경 여부를 판단한다.

```js
const a = () => {};
const b = () => {};

console.log(a === b); // false (새로운 함수가 생성되었기 때문)
```

➡ useCallback을 사용하면 함수가 새로 생성되지 않도록 **참조값을 고정**할 수 있다.

```js
const memoizedFunction = useCallback(() => {}, []);
```

위처럼 useCallback이 기존의 함수 참조를 재사용하여 불필요한 메모리 낭비를 줄일 수 있다.

## useCallback의 사용 이유 (React 최적화)

### 1. 불필요한 리렌더링 방지 (React.memo)

컴포넌트가 불필요하게 리렌더링 되는 것을 방지하기 위해 사용한다.

```js
const ChildComponent = React.memo(({ onClick }) => {
  console.log("Child Rendered!");
  return <button onClick={onClick}>Click me!</button>;
});
const Parent = () => {
  const handleClick = useCallback(() => {
    console.log("Button Clicked!");
  }, []);
  return <ChildComponent onClick={handleClick} />;
};
```

위 예시에서 `handleClick`이 `useCallback`을 사용하여 **항상 같은 참조값을 유지**하기 때문에, `React.memo`가 적용된 `ChildComponent`는 불필요한 리렌더링을 방지할 수 있다.

### 2. useEffect의 의존성 배열 최적화

useEffect의 **불필요한 재실행을 방지**할 수 있다.

```js
const fetchData = useCallback(() => {
  console.log("Fetching data...");
}, []);

useEffect(() => {
  fetchData();
}, [fetchData]); // useCallback이 없으면 fetchData가 계속 새로 생성된다.
```

위 예시에서 `useCallback`을 사용하여 `fetchData`의 **참조값을 유지**하면, `useEffect`가 불필요하게 **다시 실행되는 것을 방지**할 수 있다.

## 정리

| useCallback            | 모던 자바스크립트 특성과의 관계                                                                                                    |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| 함수 재생성 방지       | 자바스크립트의 **1급 객체 특성** 덕분에 함수가 변수처럼 다뤄지지만, 리렌더링 시 새로 생성되는데, 이때 useCallback은 이를 방지한다. |
| 참조값 유지            | **참조형 데이터의 특성** 덕분에 useCallback으로 기존 함수 참조를 재사용 가능하다.                                                  |
| 클로저 활용            | **클로저** 덕분에 기존 변수를 기억하면서도 참조값은 변경되지 않는다.                                                               |
| 불필요한 리렌더링 방지 | `React.memo`, `useEffect` 의존성 배열과 함께 사용하여 최적화 가능하다.                                                             |

➡ useCallback은 모던 자바스크립트의 **클로저, 참조형 데이터, 1급 객체** 개념을 활용하여 **불필요한 함수 재생성을 방지**하고 **리액트 최적화**에 도움을 준다.
