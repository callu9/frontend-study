# 재조정 (Reconciliation)

참고 - 공식 문서(https://ko.legacy.reactjs.org/docs/reconciliation.html)

Q. React는 DOM을 효율적으로 업데이트하기 위해 Diffing 알고리즘을 사용한다. 이때, Diffing 알고리즘과 Reconciliation의 연관성은?

## CRP (Critical Rendering Path)

: 브라우저가 렌더링하는 방식을 일컫는다. (참고 - [브라우저는 어떻게 동작하는가](https://d2.naver.com/helloworld/59361))

![webkit-rendering](https://d2.naver.com/content/images/2015/06/helloworld-59361-3.png)

1. 렌더링 엔진은 HTML 파서에 의해서 HTML을 파싱한 후에 `DOM` tree를 생성합니다.
2. 그 다음에 CSS 파서에 의해서 CSS를 파싱한 후에 `CSSOM` tree를 생성합니다.
3. HTML과 CSS를 파싱해서 만들어진 DOM tree와 CSSOM tree를 합쳐서 `render tree`를 만듭니다. (`render tree`: 화면에 표시되어야 할 모든 노드의 컨텐츠 , 스타일 정보 포함 )
4. Layout: Render Tree를 기반으로 실제 웹 페이지에 요소들의 배치를 결정하는 `layout` 작업을 합니다.
5. Paint: 배치가 완료되면 웹 페이지안에 실제 요소들을 화면에 나타나는 `paint` 작업을 실행합니다.

### 자바스크립트에 의해 DOM 수정 시 일어나는 일

- Reflow: 자바스크립트로 DOM을 변경하거나, CSS에서 위치값(width, height, margin, padding 등)을 변경하는 경우
- Repaint: 시각적 스타일만 변경하는 경우 (color, background-color, border 등)

이때, Reflow가 발생하게 되면 무조건 Repaint가 일어나며, Repaint는 단독으로 일어날 수 있다.

위 두 과정은 브라우저 연산 중 가장 비싼 연산이며, 웹 성능 악화의 주범이 되기 때문에 최소한의 CRP 과정을 통해 화면을 그려야 한다.

## React 렌더링 프로세스

현대 웹에서 유저 인터렉션이 많아지고 그에 따른 DOM 수정사항이 많아지게 되면서 Reflow, Repaint가 자주 일어나 성능적 문제가 발생하게 됐다. 

이를 해결하기 위해 리액트는 자신만의 렌더링 프로세스를 만들어서 'DOM 변경을 추적해서 하나하나 문제점을 해결하는 최적화' 과정과 같은 수고스러움을 덜어주기 위해 **가상 DOM**(Virtual DOM) 개념을 도입하게 되었다.

Virtual DOM은 아래와 같은 렌더링 프로세스를 거친다.

![react-render-and-commit](render-and-commit.png)

(참고 - [React render and commit](https://react.dev/learn/render-and-commit))

1. Trigger: 렌더링이 트리거 된다. (손님의 주문을 주방으로 전달)
2. Render: 컴포넌트를 렌더링한다. (주방에서 주문을 준비)
3. Commit: DOM에 변경사항을 커밋한다. (테이블에 주문한 요리 서빙)

### Render Phase

- 컴포넌트를 계산하고 업데이트 사항을 파악하는 단계
- React Component ➡️ React Element (Object) ➡️ Virtual DOM 생성

### Commit Phase

- 변경사항을 Virtual DOM ➡️ 실제 DOM에 반영하는 단계
- 이후 CRP를 통해 화면에 반영된다.

## DOM Update가 발생되면

![virtual-dom-updated](virtual-dom.png)

1. 변경된 UI가 반영된 Next Virtual DOM을 만든다.
2. Next Virtual DOM 기반으로 Prev Virtual DOM과 비교하여 차이점만 업데이트 한다.
	- 이때, 업데이트를 위해 차이점을 찾기 위해 사용되는 알고리즘이 비교(Diffing) 알고리즘
	- 해당 과정 전체를 재조정(Reconciliation)이라고 한다.
3. Commit Phase로 넘어가서 실제 DOM에 반영한다.

## 정리

Virtual DOM을 생성하기 위한 재조정(Reconcilation) 과정 속에 비교(Diffing) 알고리즘이 사용된다.