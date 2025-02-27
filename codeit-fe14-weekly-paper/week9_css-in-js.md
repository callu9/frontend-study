# CSS-in-JS

## 개념 및 정의

- Javascript 안에서 CSS 스타일을 작성하고 관리할 수 있는 기법
- React, Vue, Angular 등 컴포넌트 기반 프레임워크에서 컴포넌트 단위로 스타일을 관리하기 위해 도입됨
- Javascript 변수와 로직을 활용하여 동적 스타일링 가능

## 특징 및 동작 방식

- 컴포넌트 단위로 스타일을 캡슐화하여 전역 스타일 충돌을 방지
- Javascript의 모듈 시스템을 사용하여 스타일을 공유하고 재사용할 수 있음
- 동적 스타일링과 조건부 스타일링 용이
- JSX 문법과 함께 사용하여 직관적으로 스타일을 관리할 수 있음

## 예시

### 0. 주요 CSS-in-JS 라이브러리

- Styled Components
- Emotion
- JSS (Javascript Style Sheets)
- Styled JSX (Next.js 전용)
- Linaria, Panda, VanillaExtract, StyleX... (Zero-runtime CSS-in-JS)

### 1. Styled Components

```js
import styled from "styled-components";

const Button = styled.button`
  background-color: ${(props) => (props.primary ? "blue" : "gray")};
  color: white;
  padding: 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;

  &:hover {
    background-color: ${(props) => (props.primary ? "darkblue" : "darkgray")};
  }
`;

const App = () => {
  return (
    <div>
      <Button primary>Primary Button</Button>
      <Button>Secondary Button</Button>
    </div>
  );
};
export default App;
```

### 2. Emotion

```js
/** @jsxImportSource @emotion/react */
import { css } from "@emotion/react";

const buttonStyle = (primary) => css`
  background-color: ${primary ? "blue" : "gray"};
  color: white;
  padding: 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;

  &:hover {
    background-color: ${primary ? "darkblue" : "darkgray"};
  }
`;

const App = () => {
  return (
    <div>
      <button css={buttonStyle(true)}>Primary Button</button>
      <button css={buttonStyle(false)}>Secondary Button</button>
    </div>
  );
};
export default App;
```

## 장점

### 1. 컴포넌트 단위 캡슐화

- 컴포넌트 단위로 스타일이 분리되어 **스타일 유지보수 용이**하다.
- 스타일이 컴포넌트 내부에 존재하므로, 전역 스타일 충돌이 없다.
- 컴포넌트의 재사용성과 유지보수성이 높다.
- 고유한 class명을 생성하여 전역 네임스페이스 오염을 방지한다. (Styled Components)

### 2. 동적 스타일링

- props를 활용하여 **동적으로 스타일을 변경** 가능하다.
- Javascript의 조건문, 함수 등을 사용하여 복잡한 스타일도 구현할 수 있다.

### 3. 모듈화 및 재사용

- 스타일을 모듈화하고 공유하여 코드 중복을 줄일 수 있다.
- 공통 스타일을 유틸리티 함수로 관리하여 유지보수가 용이하다.

### 4. React/JSX 문법과의 호환성

- JSX와 함께 사용하여 직관적으로 스타일을 작성할 수 있다.
- 스타일과 로직을 한곳에서 관리하여 가독성이 높아진다.

## 단점

### 1. 런타임 성능 저하

- 런타임에 스타일을 생성하고 주입하기 때문에 렌더링 성능이 저하될 수 있다.
- 특히 대규모 애플리케이션에서 렌더링 성능에 영향을 미칠 수 있다.

### 2. 복잡한 설정

- SSR(Server-Side Rendering)을 지원하기 위해 설정이 복잡할 수 있다.
- Next.js등 SSR 프레임워크와 연동 시 설정 작업이 필요하다.

### 3. 빌드 사이즈 증가

- CSS가 JS에 포함되므로, 번들 크기가 증가할 수 있다.
- 스타일 캐싱이 어려워 네트워크 비용이 증가할 수 있다.

### 4. 도구 및 라이브러리 의존성

- Emotion, Styled Components 등 라이브러리 의존성이 추가된다.
- 팀 내에서 통일된 스타일링 규칙을 정의해야 일관성을 유지할 수 있다.

# Presentational & Container 디자인 패턴

## 개념 및 정의

### 1. Presentational Component

- UI 렌더링에만 집중하는 컴포넌트로, 스타일링과 마크업만 포함하고 상태관리나 비즈니스 로직이 없다.
- props를 받아서 화면에 표시하고, 이벤트가 발생하면 콜백 함수만 호출한다.
- 재사용성이 높고 디자인을 쉽게 변경할 수 있다.

```jsx
const Button = ({ text, onClick }) => {
  return <button onClick={onClick}>{text}</button>;
};
```

### 2. Container Component

- 상태관리 및 비즈니스 로직을 담당하는 컴포넌트로, 데이터를 조회 및 가공하여 Presentational Component에 전달한다.
- Redux, Context API, API 호출 등의 상태 관리 및 로직 처리를 수행한다.
- UI 로직을 분리하여 유지보수성과 테스트가 용이하다.

## 역할 및 특징

| **구분**          | **Presentational Component**             | **Container Component**                           |
| ----------------- | ---------------------------------------- | ------------------------------------------------- |
| **역할**          | UI 렌더링                                | 상태관리 및 비즈니스 로직 담당                    |
| **관심사 분리**   | 디자인 및 스타일링                       | 데이터 처리 및 비스니스 로직                      |
| **상태 관리**     | 없음 (stateless)                         | 있음 (statefull)                                  |
| **데이터 소스**   | props를 통해 데이터를 받음               | Redux, Context API, API 호출 등 <br />데이터 관리 |
| **재사용성**      | 높음 <br />(다양한 컨테이너와 조합 가능) | 낮음 <br />(특정 기능에 의존적)                   |
| **의존성**        | 독립적                                   | Presentational Component에 족송적                 |
| **테스트 용이성** | 높음 <br />(UI 스냅샷 테스트)            | 보통 <br />(로직 테스트 필요)                     |

## 사용 이유 및 장점

- 관심사의 분리: UI 렌더링과 비즈니스 로직을 분리하여 가독성 및 유지보수성이 높아진다.
- 재사용성 증가: Presentational Component는 다양한 컨테이너에서 재사용 가능하다.
- 디자인 변경에 유연: UI와 비즈니스 로직이 분리되어 디자인 변경 시 영향을 최소화할 수 있다.
- 테스트 용이성: UI와 비즈니스 로직이 분리되어 스냅샷 테스트와 로직 테스트 각각 집중할 수 있다.
