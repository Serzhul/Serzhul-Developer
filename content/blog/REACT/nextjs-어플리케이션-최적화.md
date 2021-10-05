---
title: NextJS 어플리케이션 최적화
date: 2021-10-05 16:10:49
category: React
thumbnail: { thumbnailSrc }
draft: false
---

NextJS 어플리케이션을 최적화 하는 방법으로는 다음과 같은 방법이 있다.

- Meta, Head 태그를 추가하기
- Reusing logic or Reusing Component
- 이미지 최적화

# Head data 추가하기

## Head Component 사용

- Head 컴포넌트는 컴포넌트의 내의 어떤 JSX code에서도 사용 가능한 컴포넌트. (hooks처럼 top level에서만 사용할 필요 없음)

### 사용 예시

```jsx
import Head from 'next/head'

export default function someComponents(props) {
  return (
    <div>
      <Head>
        <title>타이틀</title>
      </Head>
    </div>
  )
}
```

- Head 컴포넌트 안에 입력한 내용들은 NextJS가 랜더링 될 때, 실제 html의 head 태그 내부에 추가되는 내용들이다.
- head 안에 들어가는 tag들 (title, meta 등)을 추가할 수 있으며, 이렇게 추가 된 내용들은 검색엔진에 노출되는 역할을 해준다. (SEO)

## 동적으로 head 데이터 추가하기

- 어플리케이션을 개발하다보면, 페이지별로 다른 내용의 title과 description이 들어가야 하는 경우가 있다. 이때 Head 컴포넌트를 사용해 필요한 데이터를 template literal로 구현하면, 데이터 값이 바뀔 때마다 동적으로 head data를 추가할 수 있다.

### 사용 예시

```jsx
<Head>
  <title>Filtered Events</title>
  <meta
    name="description"
    content={`All events for ${date.month}/${date.year}.`}
  />
</Head>
```

# Reusing logic, Reusing component

- 위에서 사용한 Head 컴포넌트를 사용해 head 영역에 데이터를 추가하는 것은 유용한 방식이지만, 페이지별로 모두 head 영역 데이터를 추가해야하거나, if문에 따라 각각 다른 페이지가 랜더링 되야할 때 head에 내용을 추가하고자 한다면 Head 컴포넌트의 내용을 모든 페이지에 복사 붙여넣기 해서 사용해야 될 것이다.
- 이것은 매우 번거로운 작업이 될 수 있으므로, Reusing logic 혹은 Resuing componetn라는 방식을 사용해 이 문제를 좀 더 효과적으로 접근할 수 있다.

### Reusing Logic | component

- 로직, 혹은 컴포넌트를 재사용한다는 의미이며, react는 jsx코드를 하나의 변수로 담아 재사용할 수 있기 때문에 이 방식을 응용한 방식이라 할 수 있다.

### 사용 예시

```jsx
const pageHeadData = (
  <Head>
    <title>Filtered Events</title>
    <meta
      name="description"
      content={`All events for ${date.month}/${date.year}.`}
    />
  </Head>
)
```

- 위 코드는 Head 컴포넌트의 내용을 하나의 jsx코드 변수로 담은 방식이다. 이렇게 변수 하나로 구성해 놓으면 언제든지 재사용이 가능하므로, 좀 더 효율적으로 head 데이터를 작성할 수 있다.

### \_app.js를 통한 전역 컴포넌트 설정

- NextJS앱의 \_app.js 파일은 root 컴포넌트로서 구성은 다음과 같다.

```jsx
import Layout from '../components/layout/layout'
import '../styles/globals.css'

function MyApp({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  )
}

export default MyApp
```

- 위 코드에서 볼 수 있듯, MyApp이라는 root 컴포넌트에 Component와 pageProps라는 매개체를 전달하며 App을 구성하고 있다.
- 모든 컴포넌트는 root 컴포넌트의 Component를 통해 랜더링 되며, 여기에 Head 컴포넌트를 사용하면 모든 페이지에 동시에 노출되게 할 수 있다.

### Head 컴포넌트 사용 예시

```jsx
function MyApp({ Component, pageProps }) {
  return (
    <Layout>
      <Head>
        <meta name="viewport" content="initil-sacle=1.0, width=deivce-width" />
      </Head>
      <Component {...pageProps} />
    </Layout>
  )
}
```

## head 컴포넌트의 병합

- \_app.js를 통해 공통적으로 추가해야할 head 속성을 모든 컴포넌트에 추가하는 방식은 한 가지 문제점을 갖고 있다. 바로 NextJS는 Head 요소들의 내용이 여러가지가 있을 때 그 값을 병합해서 보여주기 때문이다.
- 따라서 Head 컴포넌트가 여러가지 중첩적으로 작용될 때 콘텐츠가 충돌할 수 있다.
- NextJS가 인식하는 것은 가장 나중에 작성된(최신) Head를 기준으로 출력해주기 때문에, 이를 고려해서 개발하는 방식이 필요하다.

## \_document.js를 통한 전역 컴포넌트 설정

- \_app.js 파일 외에도 NextJS에는 \_document.js라는 전역 설정을 할 수 있는 파일이 있다. 이 페이지는 \_app.js와 마찬가지로 pages 폴더 내에 존재해야 한다.
- \_app.js의 역할이 root 컴포넌트의 역할 (html의 body)를 한다면, \_document.js의 역할은 HTML 문서 전체의 설정을 관장한다.

### \_document.js 구조 예시

```jsx
import Document, { Html, Head, Main, NextScript } from 'next/document'

class MyDocument extends Document {
  render() {
    return (
      <Html>
        <Head />
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    )
  }
}

export default MyDocument
```

- class 기반 컴포넌트로 Document라는 컴포넌트를 상속받아 작성되며, 그 외에도 Html, Head, Main, NextScript라는 컴포넌트를 사용해 전반적인 html 문서를 구성한다.
  - 여기서 주의할 점은 위에서 next/head 패키지의 Head 컴포넌트와 next/document package의 Head는 이름은 같지만 다른 용도로 사용되는 다른 컴포넌트라는 사실이다.
  - document의 Head 컴포넌트는 \_document.js 에서만 사용된다.

# Image 최적화 하기

- 일반적으로 어플리케이션에 image를 삽입하여 사용하면 image가 갖고 있는 해상도, 용량을 그대로 사용하기 때문에 필요 이상으로 리소스를 많이 잡아 먹게 된다. 따라서 크기에 따라 리소스를 조절할 수 있도록 최적화를 해야 하는데, NextJS에서는 `**Image**` 컴포넌트를 사용하여 쉽게 image를 최적화 할 수 있는 기능을 제공한다.

## Image 컴포넌트

- Image 컴포넌트를 사용하여 image를 불러 오면 NextJS는 해당 이미지의 여러 버전을 불러오고 OS와 기기 크기에 따라 이미지를 최적화해준다. 또한 해당 이미지는 캐싱으로 저장되어, 비슷한 다른 OS나 기기에서 사용될 수 있다.

### 사용 예시

```jsx
import Image from 'next/image'

;<Image src={'/' + image} alt={title} width={250} height={160} />
```

- Image 컴포넌트는 image 태그 사용과 방법이 유사하지만, width와 height 값 설정이 추가로 필요하다.
