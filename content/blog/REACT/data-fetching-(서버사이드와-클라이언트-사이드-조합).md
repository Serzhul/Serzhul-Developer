---
title: Data fetching (서버사이드와 클라이언트 사이드 조합)
date: 2021-09-30 07:09:13
category: React
thumbnail: { thumbnailSrc }
draft: false
---

# 전통적인 React App의 문제점

- 데이터를 백엔드 서버 등에서 가져올 때, 전통적인 React App은 클라이언트 사이드에서 랜더링 해주기 때문에 데이터를 가져오는 시간까지 일정 시간의 Delay가 존재하고 이는 UX측면에서 좋지 않다.
- 또한 검색 엔진에서 웹페이지의 내용을 포함시켜야 하는 경우 (블로그 등), 클라이언트 사이드에서 랜더링하게되면 브라우저가 인식하는 html은 최소한의 구조이고 그 내용을 파악할 수 없다.

# 페이지 사전 랜더링(Page Pre-Rendering)

전통적인 React App의 문제점들을 해결해 주기 위해 NextJS에서는 Page Pre-rendering 기능을 제공한다.

## 사전 랜더링 동작 방식

- 만약 특정 페이지가 있다고 가정해보자. 사용자는 이 페이지를 방문하고 싶어한다. NextJS는 이 페이지를 사전 랜더링을 통해 반환한다.
- 전통적인 React App과 비교해 사전 랜더링을 통해 html구조와 필요한 데이터를 모두 가져오게 된다. 즉, 완성된 html페이지를 서버에서 만들고 클라이언트 쪽에 반환한다.
- NextJS에서는 한번 로드된 React 코드와 Hydration하게되는데 Hydration란 서버 사이드에서 사전 랜더링한 정적 html을 클라이언트 사이드의 자바스크립트 코드에 의해 동적으로 작동하게 만드는 방식을 말한다.
- 즉, 구성한 최소한의 핵심 데이터들을 서버 사이드에서 미리 랜더링 해오고, 상호작용이 필요한 자바스크립트 코드는 브라우저(클라이언트)에서 작동하도록 만든다.

## 사전 랜더링의 두 가지 양식

- Static Generation
- Server-side Rendering

# 정적 생성(Static Generation)

- 빌드(서버 사이드에 데이터가 준비 됐을 때) 할 때, 페이지를 사전 생성사전 생성하는 개념이다.
  - 미리 생성된 페이지들은 서버나 CDN을 통해 캐싱되고, app에서 사용된다.
  - 이렇게 미리 생성된 페이지들은 요청이 올 때마다 사용된다.
- 그렇다면 어떤 페이지를 미리 생성해야되는지, 어떤 데이터가 미리 생성된 페이지에 필요한 지 NextJS에게 말해줄 수 있을까?
  - NextJS는 페이지 컴포넌트들 내에서만 내보낼 수 있는 함수 getStaticProps를 제공한다.

```jsx
export async function getStaticProps(context) {...}
```

- getStaticProps는 promise 객체를 반환하며, 내부에 서버 사이드에서만 실행되는 코드를 작성할 수 있다.
  - 또한 함수 내부에 작성된 코드는 클라이언트 사이드에 반환되는 코드에 포함되지도 않는다. 따라서 클라이언트 사이드에서 해당 내용을 볼 수 없다.

## getStaticProps

- getStaticProps 함수는 어떠한 page 파일에도 추가될 수 있으며, 내보내서 사용해야 한다.
- NextJS는 페이지를 사전 생성 할 때 getStaticProps를 호출하고, 함수는 NextJS에게 해당 페이지가 사전 생성해야된다는 신호를 보내게 된다.

### 코드 사용 예시 1)

```jsx
function HomePage(props) {
  const { products } = props

  return (
    <ul>
      {products.map(product => (
        <li key={product.id}>{product.title}</li>
      ))}
    </ul>
  )
}

export async function getStaticProps() {
  return {
    props: {
      products: [{ id: 'p1', title: 'Product 1' }],
    },
  }
}

export default HomePage
```

- getStaticProps를 통해 Pre-fetched된 데이터를 랜더링 해주는 코드 예시이다.
- 위 코드를 실행시켜보면 코드 검사를 통해 Product 1 데이터가 개발자 툴에 보이는 것을 알 수 있는데, 이는 데이터 fetching이 서버 사이드에서 이뤄졌고, 이를 클라이언트로 반환했기 때문이라는 것을 알 수 있다.

### 코드 사용 예시 2)

```jsx
{
  "products": [
    { "id": "p1", "title": "Product 1" },
    { "id": "p2", "title": "Product 2" },
    { "id": "p3", "title": "Product 3" }
  ]
}
```

```jsx
// /pages/index.js
import path from 'path' // building Path
import fs from 'fs/promises'

function HomePage(props) {
  const { products } = props

  return (
    <ul>
      {products.map(product => (
        <li key={product.id}>{product.title}</li>
      ))}
    </ul>
  )
}

export async function getStaticProps() {
  const filePath = path.join(process.cwd(), 'data', 'dummy-backend.json') // cwd: current working directory
  const jsonData = await fs.readFile(filePath)
  const data = JSON.parse(jsonData)

  return {
    props: {
      products: data.products,
    },
  }
}

export default HomePage
```

- 이번에는 하드코딩이 아닌 JSON 파일을 만들어 불러와 랜더링 하는 예제이다.
- NodeJS의 기본 패키지인 file system에서 fs를 불러와 파일 읽는 것을 구현한다.
- 또한 path 패키지를 통해 현재 파일이 실행되는 경로를 가져올 수 있다. (코드 참조)
- 여기서 중요한 것은 `process.cwd()`에서 cwd가 가리키는 것은 pages 폴더가 아니라는 점이다.
  - getStaticProps가 실행되면, NextJS가 해당 코드를 실행하게되기 때문에 cwd는 pages 폴더가 아닌 프로젝트의 root folder로 인식한다.
- 배포 전에는 build 명령어를 통해 프로젝트를 재구성하고, start 명령어를 통해 실행하면 정상적으로 동작한다.

### 증가하는 정적 생성(Incremental Static Generation)

- getStaticProps를 통해 필요한 부분만 사전 랜더링을 할 수 있는 방법은 알았지만, 한 가지 문제가 있다. 데이터가 자주 바뀌는 경우, 매번 빌드를 다시 해줘야한다는 점이다.
- 이를 해결하기 위한 몇가지 해결책이 있다.

  - 첫 번째는 페이지를 사전 빌드하되, 일반적인 React 코드를 포함하여 useEffect로 서버에서 온 갱신된 data를 받아오는 것이다.
    - 이를 통해 새로운 데이터가 있을 때 자동으로 페이지를 업데이트 해준다.
  - 두 번째는 Incremental Static Generation로서, 페이지를 build 할 때만 정적으로 생성하는 것이 아니라 배포한 이후에도 계속해서 업데이트 하는 것이다.

    - 사전 생성된 페이지에 요청이 발생할 때마다 최대 몇 초 간격으로 재생성되도록 한다.
    - 만약 그렇게 오래된 데이터가 아니라면 재생성 할 필요가 없고, 만약 재생성 할 필요가 있다면, 생성 후 새로운 페이지를 저장한다.
    - 구현 방법은 return 객체에 revalidate 속성을 부여한다. revalidate 속성은 최대 속성 값(초 단위)마다 재생성할 것인지를 결정한다.

    ```jsx
    return {
      props: {
        products: data.products,
      },
      revalidate: 10,
    }
    ```

### notFound, redirect

- getStateProps가 반환하는 객체의 속성 값 중 props, revalidate 외에도 notFound, redirect가 있다.
- notFound는 Boolean 값으로, true인 경우 404 에러가 발생했을 때 일반적인 404 페이지가 아닌 NextJS에서 만든 페이지를 제공해준다.
- redirect는 페이지 내용을 랜더링 하는 대신, 다른 페이지로 직접 연결해준다.
  - 만약 data를 fetch하는데 실패했을 경우, 해당 페이지를 반환하는 대신 다른 페이지로 연결해주고 싶을 때 사용할 수 있다.
  - redirect 값으로는 객체가 오며 destination(redirect 할 주소) 값을 지정한다.

## 동적 파라미터 다루기

- getStaticProps 함수는 react 컴포넌트가 실행되기 전에 실행된다는 특징을 갖고 있다. 따라서 미리 페이지를 구성할 때 필요한 URL의 파라미터를 가져와 사용할 수 있다.
- context는 여러가지 key를 갖고 있는 객체인데 가장 중요한 key로 params가 있다.
  - params 동적 route에서 쓰이는 파라미터의 값들을 갖고 있다. 가령 [productId].js 란 페이지가 있고, 그 productId 값을 갖고 오고 싶다면 다음과 같이 작성할 수 있다.

```jsx
export async function getStaticProps(context) {
  const { params } = context
  const { productId } = params
}
```

- 그러나 동적 파라미터로 작성한 페이지는 NextJS에서 사전 랜더링을 자동으로 해주지 않는다. 그 이유는 동적으로 생성되는 페이지가 1개가 아니라 여러 개가 올 수 있고 NextJS가 알 수 없기 때문이다.

  - 이런 경우, 어떤 경로의 페이지가 사전 생성될지 알려줘야 하기 때문에 이를 알려주기 위한 함수 getStaticPaths이 있다.

  ```jsx
  export async function getStaticPaths() {...}
  ```

## getStaticPaths

### 코드 사용 예시

```jsx
export async function getStaticPaths() {
  return {
    paths: [
      { params: { productId: 'p1' } },
      { params: { productId: 'p2' } },
      { params: { productId: 'p3' } },
    ],
    fallback: false,
  }
}
```

- getStaticPaths는 대표적으로 2개의 속성값을 가진 객체를 반환한다.
  - paths는 사전 생성할 동적 페이지의 경로를 의미하며, 배열 안에 params 속성을 갖는 객체를 담는다. params의 속성 값은 동적으로 생성될 url 구조가 담긴다.
  - fallback은 사전 랜더링 할 페이지가 많을 때 유용한 옵션이다. fallback은 boolean 값으로, fallback이 true이면 몇 페이지만 사전 생성할 수 있다. 자주 방문하는 페이지의 경우 사전 생성을 하는 것이 성능면에서 유리하지만 그렇지 않은 페이지는 굳이 사전 생성을 해줄 필요가 없기 때문에 fallback 옵션을 통해 구분해주는 방법을 제공하는 것이다.
  - fallback을 true로 사용할 경우에는 getStaticPaths 에서 설정한 자주 사용하는 페이지가 아닐 경우 사전 랜더링이 되지 않기 때문에 오류가 발생할 수 있다. 이 때는 다른 내용(e.g 로딩페이지 등)을 반환해서 랜더링이 될 때까지 기다려야 한다.
  - fallback은 true와 false 외에도 'blocking' 값을 줄 수 있다.
    - 'blocking'은 사전 생성되지 않은 내용을 랜더링 될 때까지 기다린 다음 랜더링 해주기 때문에 true에서 발생하는 문제점을 해결해줄 수 있다. 지연 시간이 발생하긴 하지만, 로딩 페이지 같이 필요없는 페이지를 보여주는 대신 기다리는 식으로 처리해줄 수 있기 때문에 개발자의 의도에 따라 true나 blocking의 역할을 나눠서 사용할 수 있다.

# 서버 사이드 랜더링(Server-side Rendering)

- 페이지를 사전 랜더링 할 때, 모든 요청에 대해 사전 랜더링을 하거나 요청 객체(e.g 쿠키)에 접근 해야할 때가 있다.
- NextJS는 이런 경우를 위해 `real server-side code` 를 작동할 수 있도록 하는 함수를 제공한다. real server-side code란, 페에지가 실제 서버에 도착하는 요청을 관리하는 코드를 말한다. 그 함수는 getServerSideProps()이며 사용 방식은 다음과 같다.

  ```jsx
  export async function getServerSideProps() { ... }
  ```

### getServerSideProps

- getServerSideProps()는 getStaticProps()와 유사하지만 context로 접근할 수 있는 데이터나, 랜더링 시점에서 차이를 보인다.
- getServerSideProps() params 외에도 req(request), res(response) 객체에 접근이 가능하다.
- getServerSideProps는 getStaticPaths를 따로 설정해주지 않아도 된다.
  - getServerSideProps를 사용하는 컴포넌트는 서버사이드에서 매번 랜더링 되기 때문이다. 사전 랜더링을 할 필요 없고, 어떤 페이지가 사전 생성을 해야되는지 인지해야할 필요가 없다.

# 클라이언트 사이드 Data Fetching(Client-side Data Fetching)

- NextJS 프로젝트를 진행하다보면 몇몇 데이터는 사전 랜더링 될 필요가 없거나 사전 랜더링 될 수 없는 경우가 있다.
  - 데이터가 높은 빈도로 변경되는 경우(e.g 주식 데이터)
  - 유저 특화 데이터 (e.g. 유저의 최근 온라인 쇼핑 기록)
  - 부분 데이터(e.g 페이지에서 부분적으로만 사용되는 데이터)
- 이 같은 데이터들은 사전 fetching하거나 페이지를 사전 생성하는 경우 제대로 작동하지 않거나 필수값으로 사용되야 할 수 있다. 이런 경우네느 전통적인 클라이언트 사이드 data fetching 방식을 적용한다. (e.g useEffect())

## SWR(Stale-While-revalidate)

- SWR은 React hook으로 NextJS 팀에서 개발했지만, NextJS 프로젝트가 아닌 일반 react 프로젝트에서도 사용 가능하다.
- SWR은 일반적인 httpRequest를 보내는 fetch 방식과 유사하지만 몇 가지 유용한 특징들이 있다. (e.g caching, automatic revalidation, retries on error등)

### 사용 예시

```jsx
const fetcher = url => fetch(url).then(res => res.json())

const { data, error } = useSWR(url, fetcher)
```
