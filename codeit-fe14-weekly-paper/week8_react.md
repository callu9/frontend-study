# 리액트 생명주기

## 리액트 생명주기(Lifecycle)란?

- 리액트 컴포넌트는 생성, 업데이트, 소멸까지 특정한 생명주기(Lifecycle)를 가진다.
  - 클래스형 컴포넌트는 생명주기 메서드 사용해 관리 (`componentDidMount`, `componentDidUpdate` 등)
  - 함수형 컴포넌트는 `useEffect`를 활용해 관리

## 리액트 생명주기의 주요 단계

| 단계 | 설명 | 주요 메서드/훅 |
| --- | --- | --- |
| Mounting (마운트) | 컴포넌트가 처음 생성될 때 | `componentDidMount`,<br /> `useEffect(() => {}, [])` |
| Updating (업데이트) | 상태(state) 또는 props가 변경될 때 | `componentDidUpdate`,<br /> `useEffect(() => {}, [deps])` |
| Unmounting (언마운트) | 컴포넌트가 제거될 때 | `componentWillUnmount`,<br /> `useEffect(() => { return () => {...} }, [])` |

## 함수형 컴포넌트에서 생명주기 관리 (`useEffect` 활용)

1. 마운트 시 API 호출
  ```js
  import { useEffect } from 'react';

  function Example() {
    useEffect(() => {
      console.log("컴포넌트가 마운트!")
      return () => {
        console.log("컴포넌트가 언마운트!")
      }
    }, []); // 빈 배열 → 최초 1회 실행

    return <div>Example Component</div>
  }
  ```

2. 업데이트 시 실행 (의존성 배열 활용)
  ```js
  useEffect(() => {
    console.log("count 값이 변경됨: ", count);
  }, [count]); // count 값이 변경될 때 실행
  ```

3. 언마운트 시 정리 작업 (이벤트 리스너 제거 등)
  ```js
  useEffect(() => {
    const handleResize = () => console.log(window.innerWidth);
    window.addEventListener("resize", handleResize);
    return ()  => {
      window.removeEventListener("resize", handleResize); // 정리 (clean-up)
    }
  }, []);
  ```

## 리액트 생명주기의 활용 예시

| 기능 | 생명주기 단계 | 사용 예시 |
| --- | --- | --- |
| API 데이터 가져오기 | `componentDidMount`,<br /> `useEffect(() => {}, [])` | 초기 데이터 로딩 |
| 상태 변화 감지 | `componentDidUpdate`,<br /> `useEffect(() => {}, [deps])` | 특정 값이 변경될 때 실행 |
| 이벤트 리스너 정리 | `componentWillUnmount`,<br /> `useEffect(() => { return () => {...} }, [])` | 컴포넌트 제거 시 이벤트 해제 |


# 웹페이지 렌더링 방식 (CSR, SSR, SSG)

## CSR (Client-Side Rendering)

CSR (Client-Side Rendering, 클라이언트 사이드 렌더링)
- 설명: HTML을 먼저 로드한 후, 브라우저에서 JavaScript를 실행하여 데이터를 가져와 화면을 동적으로 생성하는 방식
- 특징:
  - 초기 로딩 속도가 느림 (Javascript 실행 후 렌더링 됨)
  - 이후 페이지 전환이 빠른 (SPA 방식)
  - 검색 엔진(SEO) 최적화가 어려움 (초기 HTML이 비어 있음)
- 사용 예시:
  - React, Vue 기반의 SPA (Single Page Application)
  - 대화형 웹 애플리케이션 (ex. Gmail, Facebook)

## SSR (Server-Side Rendering)

SSR (Server-Side Rendering, 서버 사이드 렌더링)
- 설명: 서버에서 미리 HTML을 생성하여 클라이언트에게 전달하는 방식
- 특징:
  - 초기 로딩 속도가 빠름 (완전한 HTML이 제공됨)
  - SEO 친화적 (검색 엔진이 HTML을 쉽게 읽을 수 있음)
  - 페이지 이동 시마다 서버 요청이 필요하므로, 사용자가 많아지면 서버 부하 발생
- 사용 예시:
  - SEO가 중요한 블로그, 뉴스 사이트 (ex. Medium, New York Times)
  - Next.js (React 기반 SSR 프레임 워크)

## SSG (Static Site Generation)

SSG (Static Site Generation, 정적 사이트 생성)
- 설명: 빌드 시점에 HTML을 미리 생성하여 배포하는 방식
- 특징:
  - 초기 로딩 속도가 가장 빠름 (미리 생성된 HTML 제공)
  - SEO 친화적 (HTML이 정적으로 제공됨)
  - 데이터가 자주 변경되지 않는 페이지에 적합
  - 실시간 데이터 반영이 어려움 (빌드 후 다시 배포해야 함)
- 사용 예시:
  - 블로그, 문서 사이트 (ex. GitHub Pages)
  - Next.js의 `getStaticProps()` 활용

## 비교

| 방식 | 초기 로딩 속도 | SEO 최적화 | 페이지 전환 속도 | 서버 부하 |
| --- | --- | --- | --- | --- |
| **CSR** | 느림 | 어렵다 | 빠름 | 낮음 |
| **SSR** | 빠름 | 좋음 | 느림 | 높음 |
| **SSG** | 가장 빠름 | 좋음 | 가장 빠름 | 없음 |

## 사용 예시

| 상황 | 적합한 렌더링 방식 |
| --- | --- |
| SPA (싱글 페이지 애플리케이션) | CSR |
| 검색 엔진 최적화 (SEO) 필요 | SSR or SSG |
| 블로그, 문서 사이트 (변경이 적은 정적 페이지) | SSG |
| 사용자 맞품 데이터가 많은 서비스 | SSR (로그인 정보에 따라 다르게 렌더링) |

## 정리

- CSR: SPA에서 사용자 경험(UX) 중요한 경우 사용
- SSR: SEO가 중요하거나, 사용자 맞춤 데이터를 즉시 반영해야 할 때 사용
- SSG: 데이터 변경이 적고 빠른 페이지 로딩이 중요한 경우 사용

Next.js 같은 프레임워크는 CSR, SSR, SSG를 혼합하여 사용할 수 있음