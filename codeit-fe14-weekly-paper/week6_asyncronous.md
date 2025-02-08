# 1. asyncronous

```js
// 1번
let num = 1;
	
// 2번
setTimeout(() => {
  num = 2;
}, 0);

// 3번
num = 3;

// 4번
console.log(num); // 3
```

자바스크립트는 코드 순서대로 진행되지 않는다. **비동기 처리**(Asynchronous Execution)를 지원하기 때문이다.
- 브라우저는 JavaScript를 실행할 때 싱글 스레드(Single Thread) 방식으로 동작하지만, Web API와 이벤트 루프(Event Loop)를 활용해 비동기 작업을 처리할 수 있음.
- 비동기 작업 예시: AJAX 요청, 타이머(setTimeout), 이벤트 리스너

## 동기(Synchronous)와 비동기(Asynchronous)

1. 동기 코드: 순서대로 실행되며, 이전 코드가 끝나야 다음 코드가 실행됨.

	```js
	console.log("A");
	console.log("B");
	console.log("C");
	// 출력: A B C
	```

2. 비동기 코드: 특정 코드가 실행될 때, 결과를 기다리지 않고 다음 코드를 실행함.

	```js
	console.log("A");
	setTimeout(() => console.log("B"), 1000);
	console.log("C");
	// 출력: A C (1초 후) B
	```


## 정리
비동기 처리 → 이벤트 루프와 콜백 함수로 실행 순서 제어

# 2. AJAX

## AJAX (Asynchronous JavaScript And XML)

- 설명: 페이지를 새로고침하지 않고 서버와 데이터를 주고받는 기술입니다.
- 특징:
	- 비동기(Asynchronous) 방식으로 동작 → 페이지가 깜빡이지 않고 업데이트 가능
	- 백그라운드에서 서버와 통신 → 사용자 경험(UX) 향상
	- JSON, XML 등의 데이터를 서버에서 받아와 웹페이지에 적용 가능
- 사용 예시:
	```js
	fetch("https://jsonplaceholder.typicode.com/posts/1")
	   .then(response => response.json()) // JSON으로 변환
	   .then(data => console.log(data)) // 데이터 출력
	   .catch(error => console.error("Error:", error));
	```
- 사용 사례:
	- 검색 자동완성 기능
	- 실시간 채팅
	- 무한 스크롤 (Infinite Scroll)

## 정리
AJAX → 새로고침 없이 서버와 통신 가능