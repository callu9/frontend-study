# 1. 자바스크립트의 this

## this 키워드

- 설명: 현재 실행 중인 컨텍스트(문맥, 환경)에 따라 다르게 동작하는 키워드입니다.
- 동작 방식
	1. 일반 함수에서 this: **전역 객체**(window)를 가리킴 
	   (use strict에서는 undefined)
		```js
		function show() {
		    console.log(this); // window (브라우저 환경)
		}
		show();
		```

	2. 객체의 메서드에서 this: 해당 객체를 가리킴
		```js
		const obj = {
		    name: "Alice",
		    show: function () {
		        console.log(this.name); // "Alice"
		    }
		};
		obj.show();
		```

	3. 화살표 함수에서 this: 화살표 함수는 this를 부모 스코프에서 가져옴
		```js
		const obj = {
		    name: "Alice",
		    show: () => {
		        console.log(this.name); // undefined (부모 스코프가 window)
		    }
		};
		obj.show();
		```

	4. 생성자 함수에서 this: 새로 생성된 객체를 가리킴
		```js
		function Person(name) {
		    this.name = name;
		}
		const user = new Person("Alice");
		console.log(user.name); // "Alice"
		```

	5. 이벤트 핸들러에서 this: 이벤트를 발생시킨 요소를 가리킴
		```js
		document.querySelector("button").addEventListener("click", function() {
		    console.log(this); // 클릭된 버튼 요소
		});
		```

# 정리
this가 가리키는 값, 즉 this 바인딩은 실행 컨텍스트에 따라 달라진다.