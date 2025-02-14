# 1. Virtual DOM

## Virtual DOM(가상 DOM)이란?

- 리액트에서 사용하는 **메모리 상의 가상 DOM**
- 실제 DOM을 직접 조작하는 것이 아닌, **가상의 DOM을 먼저 수정하고, 변경된 부분만 실제 DOM에 적용하는 방식**을 사용

## 사용 이유

- 브라우저의 실제 DOM(Document Object Model) 조작은 느린 작업

✅ **Virtual DOM 없이 직접 DOM 조작**

→ **DOM 전체가 다시 그려져 성능 저하 발생**

```js
document.getElementById("title").innerText = "Hello, World!";
```

- 리액트는 **Virtual DOM을 이용하여 최소한의 DOM 변경만 수행**해 성능을 최적화

✅ **Virtual DOM을 사용하는 리액트 방식**

→ **변경된 부분만 업데이트하여 빠른 렌더링 가능**

```js
const [text, setText] = useState("Hello");

return <h1>{text}</h1>;
```

## 동작 과정

1. 리액트가 Virtual DOM을 생성
2. 상태(state)가 변경되면 새로운 Virtual DOM을 생성
3. 이전 Virtual DOM과 비교 (= Diffing Algorithm)
4. 변경된 부분만 실제 DOM에 적용 (= Reconciliation 과정)

# 정리

- Virtual DOM의 장점
	- **성능 향상**: 최소한의 DOM 조작만 수행
	- **효율적인 렌더링**: 변경된 부분만 빠르게 업데이트
	- **코드 유지보수성 증가**: 직접 DOM을 다루지 않고 상태(state) 기반으로 UI 업데이트 가능

# 2. 리액트에서 배열을 렌더링할 때 key를 써야 하는 이유

## key란?

- 리액트에서 리스트(map())로 요소를 렌더링할 때 각 항목을 구별하기 위해 사용하는 **고유한 값**
	- React는 DOM을 효율적으로 업데이트하기 위해 Diffing 알고리즘을 사용한다. 즉, key를 컴포넌트가 고유한 식별자로서 명시하여 React가 어떤 항목이 변경, 추가, 삭제되었는지 효율적으로 판단하게 한다.
- key를 적절히 설정하지 않으면, 불필요한 DOM 업데이트가 발생하거나 렌더링 순서가 꼬일 수 있다

✅ **예제 (올바른 key 사용)**

```js
const items = ["React", "Vue", "Angular"];

return (
  <ul>
    {items.map((item, index) => (
      <li key={index}>{item}</li>
    ))}
  </ul>
);
```

## key를 사용하는 이유

1. 변경 감지 성능 향상
	- Virtual DOM이 이전 리스트와 새로운 리스트를 비교할 때 **각 항목의 고유한 key를 이용하여 변경 사항을 빠르게 찾음**.
	- key가 없으면 모든 요소를 다시 렌더링해야 하지만, key가 있으면 **변경된 항목만 업데이트** 가능.
2. 리스트 재배열 시 버그 방지
	- key가 없거나 index를 key로 사용하면, 리스트 요소의 순서가 바뀔 때 **잘못된 업데이트가 발생할 수 있음**.

✅ **올바른 key 사용 예시**

```js
const items = [
  { id: 1, text: "React" },
  { id: 2, text: "Vue" },
  { id: 3, text: "Angular" },
];

return (
  <ul>
    {items.map((item) => (
      <li key={item.id}>{item.text}</li>
    ))}
  </ul>
);
```

🚫 **잘못된 key 사용 (index 사용)**

→ **요소 순서가 변경될 때 문제 발생 가능**

```js
{items.map((item, index) => (
  <li key={index}>{item.text}</li>
))}
```

## 정리

- key를 사용하면 **리스트 업데이트 성능이 향상**되고 **불필요한 리렌더링을 방지**할 수 있음.
- 리스트가 동적으로 변경될 가능성이 있으면, **고유한 ID 값을 key로 사용해야 함**.