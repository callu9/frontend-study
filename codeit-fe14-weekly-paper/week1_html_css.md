
# 1. CSS Cascading

**CSS**(Cascading Style Sheet)의 **Cascading**(계단식 처리)이란, **여러 스타일 규칙이 동일한 요소에 적용될 때 어떤 규칙이 우선적으로 적용될지 결정하는 과정**을 말한다.

단어 Cascading은 <ins>폭포, 위에서 아래로 쏟아지는, 계단식</ins>이라는 뜻을 가진 단어로, 스타일 규칙이 순서와 우선순위에 따라 차례대로 적용되는 방식을 비유적으로 표현한 것.

- 명시도를 고려한 우선 적용되는 순서
    - `!important` (명시도와 무관하지만, 명시도에 직접 영향을 미침, 다른 선언보다 우선함)
    - 인라인 스타일 정의 (인라인 스타일은 항상 외부 스타일시트의 모든 스타일을 덮어쓰기 때문에 가장 높은 명시도를 갖는다고 생각할 수 있음)
    - 명시도
        - 아이디 선택자
        - 클래스 선택자, 속성 선택자, 가상(pseudo)클래스 선택자
        - 태그 선택자, 가상(psuedo)요소 선택자
    - 상속된 스타일
        - 상속: 부모 태그에 적용된 CSS 규칙은 자손에게도 상속된다. 모든 속성이 상속되는 건 아니고, 상속되는 속성(`color`, `font-family` 등)들이 정해져 있다. 조상 태그들에서 스타일이 모두 계산된 상태에서 우선순위를 따지는데, 가까운 조상에게 물려받은 속성일수록 우선순위가 높다.
- 스타일 시트 상호 충돌할 경우 우선 적용되는 순서
    - 웹 페이지를 만든 저자가 작성한 스타일 시트
    - 사용자가 작성한 스타일 시트
    - 브라우저에서 기본으로 제공하는 스타일시트
- 코드 상의 순서: 동일한 가중치를 갖는 규칙이 두 개 이상인 경우, 코드에서 아래 쪽에 쓴 코드일수록 우선순위가 높습니다.
 
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

### 주의 사항
- `!important` 는 가능한 사용하지 않는다. (스타일 디버깅을 어렵게 하기 때문)
- 인라인 스타일 정의도 가능한 사용하지 않는다. (스타일 디버깅을 어렵게 하기 때문)

# 2. 시맨틱 태그

## 시맨틱 태그 (Semantic Tag)

- 의미론적 태그를 뜻한다. 
- 시맨틱 태그는 태그가 가진 목적이나 역할을 나타내기 때문에, 태그 사용만으로도 의미를 쉽게 파악할 수 있다.

## 시맨틱 태그 사용 시 이점

- 검색 엔진 최적화 (Search Engine Optimization = SEO)
- 웹 접근성(Web Accessibility = A11y)
    - 시각장애 있는 사용자가 스크린 리더를 사용하여 페이지를 탐색할 때 도움을 준다.
- 동료 개발자가 코드를 읽고 의도 파악하는데 용이하다.

## 주의사항

시맨틱 태그에 너무 집착하면 개발 생산성이 떨어지기 때문에, `div` 태그로 개발하고 필요할 때 시맨틱 태그로 바꿔 나가는 것이 효율적일 수 있다.

## 참고 자료

https://www.semrush.com/blog/semantic-html5-guide/

# 3. font-size 상대적 단위

- 웹 페이지에 절대 단위(`px`)를 사용할 경우 사용자의 브라우저 font-size 변경을 반영할 수 없다.
- 따라서, 브라우저 font-size 변경을 반영하기 위해서는 상대적 단위( **`%`, `rem`, `em`** )를 사용해야 한다.
- `em` 은 하나의 요소에 폰트 크기를 지정하고, 자식 요소에서 `em` 을 적용하면 두 요소의 폰트 크기가 다르게 적용된다. 따라서 요소가 깊이 중첩될 수 있을 때는 폰트 크기 단위로 `em` 을 사용하면 CSS를 보는 것만으로 개발자가 어떤 크기인지 예측하기 어렵다.
	- 참고: https://codepen.io/withyj/pen/abaXRme
- `rem` 은 항상 루트 폰트 사이즈를 참조한다. 일반적인 브라우저 기본 폰트 크기가 `16px`이고 이를 이용하면 사용자가 브라우저 폰트 크기 설정을 변경하지 않는 일반적인 경우, `1rem` 은 `16px` 크기라 보고 스타일링 하면 된다.
	- 참고: [Figma CSS 변수 rem 환산 설정 방법](https://developer.mozilla.org/ko/docs/Web/CSS/Using_CSS_custom_properties)
	- 참고: [REM 단위의 계산에 영항을 미치는 요인](https://blog.jeongtae.com/rem-%EA%B3%A0%EC%B0%B0#rem-%EB%8B%A8%EC%9C%84%EC%9D%98-%EA%B3%84%EC%82%B0%EC%97%90-%EC%98%81%ED%95%AD%EC%9D%84-%EB%AF%B8%EC%B9%98%EB%8A%94-%EC%9A%94%EC%9D%B8)

# 4. position

## position 속성 종류

- `static`
- `relative`
- `absolute`
- `fixed`
- `sticky`

## 각 속성들의 특징

- `static`은 position의 기본 값이며, 이 경우 원래 있어야 할 위치인 HTML에 작성된 순서 그대로 브라우저 화면에 표시됩니다.
- `relative` 는 요소의 원래 위치를 기준으로 상대적으로 배치합니다. 이때 요소의 원래 자리는 그대로 차지하고 있습니다. `top`, `bottom`, `left`, `right` 속성을 이용해서 요소의 원래 위치 기준 이동하도록 설정할 수 있습니다.
- `absolute` 는 가장 가까운 포지셔닝(`static` 이 아닌 position 속성 값)이 된 조상 요소를 기준으로 배치됩니다. 이때 글의 흐름에서 완전히 빠져서, 요소의 원래 자리는 차지하지 않습니다. 보통 상위 요소의 position 속성을 `relative` 로 지정하여 배치할 기준을 잡고 사용합니다.
- `fixed` 는 브라우저 전체 화면을 기준으로 고정된 배치입니다. `top`, `bottom`, `left`, `right` 속성은 브라우저의 상, 하, 좌, 우에서 해당 요소가 얼마나 떨어져 있는지를 결정합니다. 글의 흐름에서 완전히 빠져서, 요소의 원래 자리는 차지하지 않습니다. 내비게이션을 만들 때 많이 사용하는데, 요소의 원래 자리를 차지하지 않기 때문에 요소간 겹치지 않도록 마진을 넣어주기도 합니다.
- `sticky` 는 `static` 처럼 원래 위치에 배치해 있다가, 정해진 위치에 브라우저가 스크롤되면 그때부터 `fixed`처럼 고정되어 배치됩니다. 기본적으로는 `static` 처럼 배치하기 때문에 요소의 원래 자리를 차지합니다. `top`, `bottom`, `left`, `right` 설정이 필요하고, 가장 가까운 scroll되는 조상을 기준으로 배치 합니다.

## 참고 자료

- [position - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/ko/docs/Web/CSS/position)
- [https://chenhuijing.com/blog/understanding-positioning-in-css/#👾](https://chenhuijing.com/blog/understanding-positioning-in-css/#%F0%9F%91%BE)