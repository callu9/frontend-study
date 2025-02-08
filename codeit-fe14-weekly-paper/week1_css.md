
# 1. CSS Cascading

**CSS**(Cascading Style Sheet)의 **Cascading**(계단식 처리)이란, **여러 스타일 규칙이 동일한 요소에 적용될 때 어떤 규칙이 우선적으로 적용될지 결정하는 과정**을 말한다.

단어 Cascading은 <ins>폭포, 위에서 아래로 쏟아지는, 계단식</ins>이라는 뜻을 가진 단어로, 스타일 규칙이 순서와 우선순위에 따라 차례대로 적용되는 방식을 비유적으로 표현한 것.

## 1-1. Cascading의 기본 원리

### 우선순위(Priority)

CSS 규칙이 적용될 때, 특정 요소에 여러 규칙이 겹치는 경우 우선순위에 따라 어떤 규칙이 적용될지를 결정합니다. 우선순위는 아래와 같은 순서로 평가됩니다.

1. 인라인 스타일 (HTML 요소 안에 직접 지정된 스타일)
	```html
	<div style="color: red;">
	```

2. ID 선택자 
	```css
	#header { color: blue; }
	```

3. 클래스, 속성, 의사클래스 선택자
	```css
	.btn { color: green; }
	```

4. 태그 선택자 
	```css
	p { color: black; }
	```

5. 전역 선택자 
	```css
	* { margin: 0; }
	```

- 예시: 아래의 경우, `style="color: red;"`이 인라인 스타일로 가장 우선순위가 높다.

```html
<div id="box" class="container" style="color: red;">
    Hello World!
</div>
```

### 특이도(Specificity)

CSS 선택자의 구체성에 따라 우선순위가 결정된다. 
1. 인라인 스타일:  가장 높은 특이도 (ex. `style=""`은 항상 이김)
2. ID 선택자 : 1점 (ex. `#id`)
3. 클래스/속성/의사클래스 선택자 : 0.1점 (ex. `.class, [attr=value], :hover`)
4. 태그 선택자: 0.01점 (ex. `h1, p`)

- 예시: `#main`이 가장 우선순위가 높아 적용된다.

```css
p { color: black; }           /* 특이도: 0.01 */
.text { color: green; }       /* 특이도: 0.1 */
#main { color: blue; }        /* 특이도: 1 */
```

### 소스코드의 위치

동일한 특이도를 가진 규칙이라면 나중에 선언된 스타일이 적용된다.
- 예시:
```css
p { color: black; }
p { color: red; } /* 나중에 선언된 규칙이 적용됨 */
```

### 중요 표시(`!important`)

스타일 뒤에 `!important`를 추가하면, 해당 규칙이 무조건 우선 적용
- 예시:
```css
p { color: black !important; }
p { color: red; }
```

## 1-2. Cascading의 의미와 중요성

Cascading은 CSS가 겹칠 때 충돌을 해결하는 방식이다. 따라서 Cascading 덕분에 웹 개발자는 다양한 스타일 규칙을 쉽게 조정할 수 있고, 효율적인 스타일 관리가 가능하다.

비유: “여러 규칙이 경쟁할 때, 누가 가장 강한지를 판별하는 토너먼트”와 같다.
