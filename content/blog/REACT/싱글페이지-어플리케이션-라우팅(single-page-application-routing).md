---
title: 싱글페이지 어플리케이션 라우팅(Single-Page Application Routing)
date: 2021-10-08 09:10:65
category: React
thumbnail: { thumbnailSrc }
draft: false
---

# 라우팅이란 무엇인가?

- 일반적으로 웹사이트에서 다른 경로의 URL을 불러오면 다른 페이지로 연결된다. 즉 URL을 변경하면 우리가 보고자 하는 페이지의 내용들이 바뀐다.
- 즉 이처럼 URL에 따라 경로를 추적하여 경로에 맞는 페이지를 보여주는 것을 라우팅이라 할 수 있다.
- 하지만 기존의 웹사이트와 달리 React App은 SPA이기 때문에 하나의 페이지로만 구성되어있다. 만약 React가 여러 페이지로 구성되어있다면 state 값도 사용할 수 없을 것이고(다시 랜더링 되기 때문에), 기존의 로직들도 제대로 동작하지 않을 것이기 때문이다.
- 따라서 SPA에서 라우팅을 구현하기 위해서는 각각의 HTML을 구성하는 파일들을 서버에 저장하거나, 서버에서 동적으로 생성하여 저장하고 다른 URL에 따라 요청하는 방식으로 구현할 수 있다.

# SPA 빌딩하기

- 복잡한 UI를 빌딩할 때, 일반적으로 SPA를 사용한다.
  - SPA는 단 하나의 HTML의 request&response를 가진다.
- 페이지(URL)가 바뀜으로써 클라이언트 사이트 코드들을 제어할 수 있다.
  - 새로운 HTML 파일을 가져와 랜더링 하지 않으면서 보이는 내용들을 바꾼다.
- 실제로 http에 request 요청을 보내는 것이 아니라 라우팅을 통해 해당 페이지(URL)에 나타나야할 콘텐츠를 Javascript를 통해 제어해주는 방식으로 구현된다.

# React router 사용하기

- React에서 라우팅을 구현하는데 가장 일반적인 방법은 react router를 사용하는 것이다.

### React router 설치 (react-router가 아니라 react-router-dom)

```bash
npm install react-router-dom
```

## 메소드 및 모듈 컴포넌트들

### BrowserRouter

최상위 어플리케이션을 감싸서 react-router를 사용하도록 만들어준다.

### 사용 예시 코드

```jsx
import { BrowserRouter } from 'react-router-dom'

import ReactDOM from 'react-dom'

import './index.css'
import App from './App'

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
)
```

### Route

각 컴포넌트들을 감싸 path property에 따라 해당 URL에 해당하는 주소가 오면 해당 컴포넌트를 랜더링 하도록 만들어줌

### 사용 예시 코드

```jsx
import { Route } from 'react-router-dom'
import Welcome from './components/Welcome'
import Products from './components/Products'

function App() {
  return (
    <div>
      <Route path="/welcome">
        <Welcome />
      </Route>
      <Route path="/products">
        <Products />
      </Route>
    </div>
  )
}

export default App
```

### Link

html의 anchor 태그 대신 react-router에서 제공되는 모듈로, 해당 요소 클릭시 to 프로퍼티에 해당하는 URL로 전환해주는 역할을 한다. 이때, a태그는 페이지가 새롭게 랜더링 되는데 반해 Link는 component만 전환된다.

### 사용 예시 코드

```jsx
import { Link } from 'react-router-dom'

const MainHeader = () => {
  return (
    <header>
      <nav>
        <ul>
          <li>
            <Link to="/welcome">Welcome</Link>
          </li>
          <li>
            <Link to="/products">Products</Link>
          </li>
        </ul>
      </nav>
    </header>
  )
}

export default MainHeader
```

### NavLink

Link과 기본적으로 동일하지만, 추가로 현재 활성화된 anchor item에 CSS를 적용해준다.

```jsx
import { NavLink } from 'react-router-dom'
import classes from './MainHeader.module.css'

const MainHeader = () => {
  return (
    <header className={classes.header}>
      <nav>
        <ul>
          <li>
            <NavLink activeClassName={classes.active} to="/welcome">
              Welcome
            </NavLink>
          </li>
          <li>
            <NavLink activeClassName={classes.active} to="/products">
              Products
            </NavLink>
          </li>
        </ul>
      </nav>
    </header>
  )
}

export default MainHeader
```

### Sub Page

Route를 사용할때 해당 URL뒤에 `:` 을 사용해서 특정값을 할당해줌 그 후에 해당 component(subPage 컴포넌트에)에 가서 useParams()를 사용해 특정값에 해당하는 값을 가져오면 URL에 입력된 값을 불러올 수 있다.

### 사용 예시 코드

```jsx
// Link Component
;<Route path="/product-detail/:productId">
  <ProductDetail />
</Route>

// subPage Component

import { useParams } from 'react-router-dom'

const ProductDetail = () => {
  const params = useParams()

  console.log(params.productId)

  return (
    <section>
      <h1>Product Detail</h1>
      <p>{params.productId}</p>
    </section>
  )
}

export default ProductDetail
```

Router에서 매칭한다는 의미는 어떤 주소로 시작하느냐의 의미를 갖는다. 즉, Router에서 추가로 url을 계속추가해도 시작점이 같기 때문에 새롭게 랜더링 되지 않고 남아있다.
