---
title: NextJS 페이지 구성하기 (파일 기반 라우팅)
date: 2021-09-29 16:09:13
category: React
thumbnail: { thumbnailSrc }
draft: false
---

# 코드 기반 라우팅 vs 파일 기반 라우팅

- 전통적인 React app은 React-router 라이브러리를 사용해 Routing을 구현했다. 이 방식은 `Code-based Routing` 방식으로 JSX, JS code를 작성해 routes를 정의하고, 페이지 구조를 만들었다.
- 그러나 NextJs에서는 이 방식 대신 `File-based Routing` 방식을 채용 했다.
  - react 컴포넌트 파일을 만들면 NextJS가 폴더 구조(pages 폴더 내부의 )로부터 route를 유추해서 구현한다.

# 파일 기반 라우팅 동작 방식

- 먼저 최상단의 pages 폴더는 기본적으로 index.js 파일을 가지며, 같은 레벨의 다른 이름을 가진 js 파일을 가질 수 있다.
  - pages의 index.js는 메인 시작 페이지라고 할 수 있다. 도메인 뒤에 아무런 내용도 없을 경우, 이 파일이 읽힌다고 이해하면 된다.
  - 같은 레벨의 다른 이름을 가진 js 파일은 그 이름이 자체가 route가 된다. 가령, about.js라는 파일이 있으면 domain 뒤에 `/about` 형태로 입력했을 때, about.js의 내용이 랜더링 된다고 이해할 수 있다.
- pages 하위의 폴더들은 각각의 route에 해당하는 이름을 갖게되며, 마찬가지로 index.js 파일을 갖고 있다. 추가로, route를 동적으로 구성하고 싶을 때는 `[]` 안에 이름을 정의한 컴포넌트 파일을 만들어서 구현할 수도 있다.
  - 하위 폴더의 index.js 파일은 폴더 내의 모든 page를 대표하는 page라고 할 수 있다. 폴더명이 route가 된다고 할 수 있다.
  - `[]` 안에는 보통 동적으로 구성하는 상세 페이지의 변수명을 넣는다. 가령 여러 상품을 갖는 페이지를 만든다고 할 때, 그 상품들에겐 고유의 id가 있을 것이고, 해당 상품의 상세페이지를 구현한다고 하면 `[id].js` 로 구현할 수 있는 식이다. route 주소는 `/products/1` , `/products/2` ... 이런 방식으로 구현될 수 있다.
  - / 뒤에 오는 값은 개발자가 정의해도 되지만, 만약 같은 이름을 가진 컴포넌트 파일이 있을 경우 그 파일을 우선적으로 읽는다.
    - 가령, portfolio 폴더 안에 list.js 파일과, [id].js 파일이 있다고 하자. list.js가 없을 때는 URL에 `portfolio/list` 를 하면 [id].js 컴포넌트가 읽어졌겠지만, list.js가 있으면 같은 이름일 때는 컴포넌트 파일을 우선순위로 읽기 때문에 list.js 컴포넌트 내용이 랜더링 된다.
- 동적으로 페이지를 구성할 때, URL로 입력된 값을 가져오기 위해서 next/router 패키지에 있는 useRouter hook을 사용할 수 있다.

  ```jsx
  import { useRouter } from 'next/router'
  ```

  - useRouter로 가져올 수 있는 값들 중 pathname과 query가 있다.

    - pathname은 []로 구성된 컴포넌트의 path 경로를 보여준다. 가령 portfolio 폴더의 [projectid].js의 파일이 있고, url을 /portfolio/1 이라고 입력했다고 하면, 화면 상에는 [projectid].js 의 내용이 표시되고 pathname에는 `/portfolio/[projectid]` 값을 갖는다.
    - query는 url에서 경로의 값을 갖는 객체로서 위의 사례의 경우 query 값은 다음과 같다.

    ```jsx
    /* 
    	 /portfolio/1 페이지에서 출력
    */

    const router = useRouter()

    console.log(router.pathname)
    // /portfolio/[projectid]

    console.log(router.query)

    // {projectid: '1'}
    ```

    - 위 사레와 같이 url로 특정 값을 갖고 올 수 있기 때문에 백엔드 서버에 특정 데이터를 fetch 한다고 할 때 router.query.projectid 값을 갖고 요청할 수 있다.
    - 이런 구조는 폴더 안에 새로운 폴더를 만드는 방식으로 몇 겹이고 만들 수 있는데, 그 때마다 []을 사용해 구성할 경우 []값에 해당하는 내용들은 query 객체안에 포함되어, url의 특정값을 가져오려고 할 때 유용하게 이용할 수 있다.
    - [... 이름] 방식으로 사용할 수도 있는데 이 경우에는 query객체에 해당 경로에 들어가 있는 모든 값들을 배열 형태로 가져온다.

# Link 걸기

- 일반적으로 URL간 이동을 위해선 anchor tag등을 활용해 다른 페이지로 이동하는 방식을 사용했었다. 그러나 이 방식은 새로운 요청을 통해 페이지 전체를 새롭게 갱신하기 때문에 상태 값들이 보존되지 않는다는 단점이 있다.
- React에서는 Link 컴포넌트를 사용해 이를 방지하고 링크로 이동할 수 있도록 구현했고, NextJS에서도 마찬가지로 Link 컴포넌트를 사용할 수 있다.

```jsx
import Link from 'next/link'

export default function HomePage() {
  return (
    <div>
      <h1>My name is Serzhul</h1>
      <ul>
        <li>
          <Link href="/portfolio">Portfolio</Link>
        </li>
      </ul>
    </div>
  )
}
```

- Link 컴포넌트는 기본적으로 링크의 주소인 href prop을 갖고 있으며 그 외에도 몇가지 prop 들이 존재한다.
  - 가령, replace prop은 해당 링크로 이동하는 것이 아니라 현재 페이지를 주소의 페이지로 교체해준다. 따라서 뒤로 돌아가기가 불가능하다.

### 동적으로 Link 리스트 구현하는 예시

```jsx
function ClientsPage() {
  const clients = [
    { id: 'serzhul', name: 'serzhul' },
    { id: 'manu', name: 'manuel' },
  ]

  return (
    <div>
      <h1>Clients Page</h1>
      <ul>
        {clients.map(client => (
          <li key={client.id}>
            <Link
              href={{
                pathname: '/clients/[id]',
                query: { id: client.id },
              }}
            >
              {client.name}
            </Link>
          </li>
        ))}
      </ul>
    </div>
  )
}
```

### 404 페이지 커스텀하기

- pages 폴더 최상위에 404.js 파일을 구성해 놓으면 자신만의 404 커스텀 페이지를 만들 수 있다.

# 요약

## 파일 기반 라우팅 vs 코드기반 라우팅

### 파일 기반 라우팅(NextJS)

- 추가 보일러플레이트 코드가 필요하지 않음
- 직관적인 시스템
- 파일 + 폴더 구조(pages 폴더 내의)가 라우팅에 영향을 미침
- 네비게이션은 Link 컴포넌트로 작동함

### 코드 기반 라우팅(React + react-router)

- 보일러플레이트 코드가 세팅되야함 (Switch, Route 등)
- 이해하기 쉽지만, 새로운 컴포넌트와 내용이 포함되어 있음
- 파일 + 폴더를 세팅할 필요가 없음
- 네비게이션은 Link 컴포넌트로 작동함
