# React Router

-  React 어플리케이션에서 라우팅을 관리하기 위한 라이브러리
- 페이지 이동 없이 URL만 변경하며 필요한 컴포넌트만 렌더링해주는 역할을 한다.
## React Router 사용 이유
1. SPA(Single Page Application) 구현
	- 리액트는 SPA 구조로, 페이지 리로드 없이 URL에 따라 하나의 HTML 파일에서 자바스크립트를 통해 여러 페이지 혹은 컴포넌트를 렌더링하는 것이다.
	- SPA와 React Router의 연관성: SPA는 페이지 전환 시 전체 페이지 리로드 없이 **필요한 부분만 업데이트**하는 웹 어플리케이션 방식으로, 리액트는 SPA 방식으로 동작하기 때문에 URL 변경 시에도 리로드 없이 업데이트를 위해서는 라우팅 시스템이 필요하다. 따라서, React Router 를 통해 SPA의 특성을 최대한 활용하면서 원활한 사용자 경험을 제공할 수 있다.
2. 브라우저 URL 관리
       - URL를 통해 현재 위치를 직관적으로 표현할 수 있다. (ex. /home, /about, …)
3. 컴포넌트 기반 설계
    - URL에 따라 보여줄 컴포넌트를 모듈화하여 관리할 수 있다.
    - 따라서 유지보수성과 재사용성을 높일 수 있다.

## React Router 작동 원리
- History API를 사용해 페이지 리로드 없이 URL을 변경한다.
- 브라우저의 URL 변경사항에 따른 서버 요청(GET을 통한 HTML 파일 다운로드) 없이, URL에 맞는 컴포넌트를 렌더링하여, 사용자에게 페이지 리로드 없이 새로운 화면이 나타난 것과 같은 느낌을 준다.

## 사용 예시
```js
import React from "react";
import { BrowserRouter as Router, Route, Routes, Link } from "react-router-dom";
import Home from "./Home";
import About from "./About";
import Product from "./Product";

function App() {
  return (
    <Router>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/product/1">Product 1</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/product/:id" element={<Product />} />
      </Routes>
    </Router>
  );
}

export default App;
```

## 참고

### **라우팅**(Routing)

: URL에 따라 사용자가 보게 될 화면이나 페이지를 결정하는 것을 말한다. 페이지 리로드 방지(SPA 방식 사용)하며 사용자 경험을 향상시키는 역할을 한다.

### History API
: 브라우저 내장 API로, **페이지 리로드 없이 URL을 변경**하거나 **브라우저 앞/뒤로 가기**를 관리할 수 있다. (ex. pushState, replaceState등)
```js
window.history.pushState({}, '', '/about');
//URL은 /about으로 변경되지만, 페이지 리로드는 발생하지 않음.

window.history.replaceState({}, '', '/contact');
// URL은 /contact로 바뀌지만 히스토리 기록은 추가되지 않음.
```