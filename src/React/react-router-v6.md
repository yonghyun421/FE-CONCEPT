# react-router-v6

## 리액트 라우터 적용 및 기본 사용법

프로젝트 생성 및 라이브러리 설치 -> 페이지를 만들고 이동해보기 -> URL 파라미터와 쿼리스트링 사용해보기
-> 중첩된 라우트 구현하기 -> 리액트 라우터의 부가기능 사용해보기

### 프로젝트 생성 및 라이브러리 설치

```bash
yarn create-react-app 프로젝트명
cd 프로젝트명
yarn add react-router-dom
```

### 프로젝트에 라우터 적용

프로젝트에 리액트 라우터를 적용할 때는 src/index.js 파일에서 react-router-dom에 내장되어 있는 BrowserRouter라는 컴포넌트를 사용하여 감싸면 된다. 이 컴포넌트는 웹 애플리케이션에 HTML5의 History API를 사용하여 페이지를 새로 불러오지 안혹도 주소를 변경하고 현재 주소의 경로에 관련된 정보를 리액트 컴포넌트에서 사용할 수 있도록 해준다.

```js
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import { BrowserRouter } from "react-router-dom";

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById("root")
);
```

### 페이지 컴포넌트 만들기

src 디렉터리에 pages 경로를 만들고, 그 안에 페이지 컴포넌트 파일들을 생성한다.

### Route 컴포넌트로 특정 경로에 원하는 컴포넌트 보여주기

사용자의 브라우저 주소 경로에 따라 우리가 원하는 컴포넌트를 보여주기 위해서 Route 라는 컴포넌트를 통해 라우트 설정을 해주어야 한다.
Route 컴포넌트는 다음과 같이 사용한다.

```js
<Route path='주소 규칙' element={보여 줄 컴포넌트 JSX} />
```

그리고 Route 컴포넌트는 Routes 컴포넌트 내부에서 사용되어야 한다.

### Link 컴포넌트를 사용하여 다른 페이지로 이동하는 링크 보여주기

리액트 라우터를 사용하는 프로젝트에서 a 태그를 바로 사용하면 안된다. 왜냐하면 a 태그를 클릭하여 페이지를 이동할 때 브라우저에서는 페이지를 새로 불러오게 되기 때문이다.

Link 컴포넌트 역시 a 태그를 사용하긴 하지만, 페이지를 새로 불러오는 것을 막고 History API를 통해 브라우저 주소의 경로만 바꾸는 기능이 내장되어 있다.

Link 컴포넌트는 다음과 같이 사용한다.

```js
<Link to="경로">링크 이름</Link>
```

## URL 파라미터와 쿼리스트링

- URL 파라미터 예시 : /profile/yonghyun
- 쿼리스트링 예시 : /articles?\*\*page=1&keyword=react
  URL 파라미터는 주소의 경로에 유동적인 값을 넣는 형태고, 쿼리 스트링은 주소의 뒷부분에 ? 문자열 이후에 key=value 로 값을 정의하며 & 로 구분을 하는 형태이다.

주로 URL 파라미터는 ID 또는 이름을 사용하여 특정 데이터를 조회할 때 사용하고, 쿼리스트링은 키워드 검색, 페이지네이션, 정렬 방식 등 데이터 조회에 필요한 옵션을 전달할때 사용한다.

### URL 파라미터

URL 파라미터는 useParams 라는 Hook을 사용하여 객체 형태로 조회할 수 있다. URL 파라미터의 이름은 라우트 설정을 할 때 Route 컴포넌트의 path props를 통하여 설정한다.

### 쿼리스트링

쿼리스트링을 사용할 때는 URL 파라미터와 달리 Route 컴포넌트를 사용할 때 별도로 설정해야되는 것은 없다.

#### useLocation

location 객체를 반환하며 이 객체는 현재 사용자가 보고있는 페이지의 정보를 지니고 있다.

- pathname: 현재 주소의 경로(쿼리스트링 제외)
- search: 맨 앞의 ? 문자 포함한 쿼리스트링 값
- hash: 주소의 # 문자열 뒤의 값 (주로 History API 가 지원되지 않는 구형 브라우저에서 클라이언트 라우팅을 사용할 때 쓰는 해시 라우터에서 사용한다.)
- state: 페이지로 이동할때 임의로 넣을 수 있는 상태 값
- key: location 객체의 고유 값, 초기에는 default 이며 페이지가 변경될때마다 고유의 값이 생성된다.

쿼리스트링은 location.search 값을 통해 조회를 할 수 있다.

쿼리스트링 값이 ?detail=true&mode=1 과 같은 형태로 표시되고 있을 때 이 문자열에서 앞에 있는 ? 로 지우고 & 문자열로 분리한뒤 key와 value를 파싱하는 작업을 해야하는데 이 작업은 보통 npm에서 qs 또는 querystring 패키지를 설치해서 저리할 수 있다.

쿼리스트링을 따로 파싱까지 해야된다면 번거로울수도 있는데, 리액트 라우터에서는 v6부터 useSearchParams 라는 Hook을 통해 쿼리스트링을 더욱 쉽게 다룰 수 있게 됐다.

#### useSearchParams

useSearchParams는 배열 타입의 값을 반환하며, 첫번째 원소는 쿼리파라미터를 조회하거나 수정하는 메서드들이 담긴 객체를 반환한다. get 메서드를 통해 특정 쿼리파라미터를 조회할 수 있고, set 메서드를 통해 특정 쿼리파라미터를 업데이트 할 수 있다. 만약 조회시에 쿼리파라미터가 존재하지 않는다면 null로 조회된다. 두번째 원소는 쿼리파라미터를 객체형태로 업데이트할 수 있는 함수를 반환한다.

쿼리파라미터를 사용할 때 주의할점은 쿼리파라미터를 조회할 때 값은 무조건 문자열타입이라는 것이다. 즉 true 또는 false 값을 넣게 된다면 값을 비교할 때 꼭 'true'와 같이 따옴표로 감싸서 비교를 해야하고 숫자를 다루게 된다면 parseInt를 사용하여 숫자타입으로 변환을 해야 한다.가나

## 중첩된 라우트

### 공통 레이아웃 컴포넌트

중첩된 라우트와 Outlet은 페이지끼리 공통적으로 보여줘야 하는 레이아웃이 있을때도 유용하게 사용할 수 있다.
예를 들어 Home, About, Profile 페이지에서 상단에 헤더를 보여줘야 하는 상황에서 Header 컴포넌트를 따로 만들어두고 각 페이지 컴포넌트에서 재사용을 하는 방법도 있겠지만 중첩된 라우트와 Outlet을 활용하여 구현을 할 수도 있다. 중첩된 라우트를 사용하는 방식을 사용하면 컴포넌트를 한번만 사용해도 된다는 장점이 있다.

### index props

Route 컴포넌트에는 index 라는 props가 있다. 이 props는 path='/'와 동일한 의미를 가진다.

## 리액트 라우터 부가기능

리액트 라우터에는 웹 애플리케이션에서 라우팅에 관련된 작업을 할 때 사용할 수 있는 유용한 API들을 제공한다.

### useNavigate

useNavigate는 Link 컴포넌트를 사용하지 않고 다른 페이지로 이동을 해야 하는 상황에 사용하는 Hook이다.

navigate 함수를 사용할 때 파라미터가 숫자 타입이라면 앞으로 가거나, 뒤로 간다. 예를 들어서 navigate(-1)을 하면 한 번 뒤로가고 navigate(-2)를 하면 두 번 뒤로간다.
반대로 navigate(1)을 하면 앞으로 한 번 간다. 물론 뒤로가기를 한번 한 상태여야 한다.

다른 페이지로 이동을 할 때 replace라는 옵션이 있는데 이 옵션을 사용하면 페이지를 이동할 때 현재 페이지를 페이지 기록에 남기지 않는다.

### NavLink

NavLink 컴포넌트는 링크에서 사용하는 경로가 현재 라우트의, 경로와 일치하는 경우 특정 스타일 또는 CSS 클래스를 적용하는 컴포넌트이다.

이 컴포넌트를 사용할 때 style 또는 className을 설정할 때 { isActive: boolean }을 파라미터로 전달받는 함수 타입의 값을 전달한다.

### NotFound 페이지 만들기

이 페이지는 사전에 정의되지 않는 경로에 사용자가 진입했을 때 보여주는 페이지이다. 즉, 페이지를 찾을 수 없을 때 나타나는 페이지이다.

- 는 wildcard 문자인데 이는 아무 텍스트나 매칭한다는 뜻이다. 이 라우트 엘리먼트의 상단에 위치하는 라우트들의 규칙을 모두 확인하고, 일치하는 라우트가 없다면 이 라우트가 화면에 나타나게 된다.

### Navigate 컴포넌트

Navigate 컴포넌트는 컴포넌트를 화면에 보여주는 순간 다른 페이지로 이동을 하고 싶을 때 사용하는 컴포넌트이다. 즉 페이지를 리다이렉트 하고 싶을 때 사용한다. 예를 들어서, 사용자의 로그인이 필요한 페이지인데 로그인을 안했다면 로그인 페이지를 보여주어야 하는 상황에 사용할 수 있다.