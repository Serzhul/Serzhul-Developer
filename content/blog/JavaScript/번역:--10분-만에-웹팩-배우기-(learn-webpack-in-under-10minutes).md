---
title: 번역) 10분 만에 웹팩 배우기 (Learn webpack in under 10minutes)
date: 2021-10-26 13:10:78
category: JavaScript
thumbnail: { thumbnailSrc }
draft: false
---

출처 : [https://betterprogramming.pub/learn-webpack-in-under-10-minutes-efe2b2b10b61](https://betterprogramming.pub/learn-webpack-in-under-10-minutes-efe2b2b10b61)

# 웹팩이란 무엇인가?

> "웹팩은 현대 자바스크립트 어플리케이션을 위한 정적 모듈 번들러 입니다."

자바스크립트 개발자로서 우리는 모듈이 무엇인지 알고 있지만, 웹팩에서의 모듈은 조금 다릅니다. 그 구성은 다음과 같습니다.

- **ES modules** — `import` 구문
- **Common JS modules** — `require()` 구문
- **AMD modules** — `define` 과 `require` 구문
- **CSS imports** — css/sass/less 파일들 안에서 사용되는  `@import` 구문
- **Image URLs** — `url(...)` 또는 `<img src="..." />`

웹팩은 이러한 다른 종류의 모듈들을 모두 자바스크립트 코드로 합쳐서 import 할 수 있도록 해줍니다.

# 웹팩을 꼭 배워야 할까?

오늘날, 대부분의 어플리케이션은 React와 Vue혹은 다른 라이브러리를 통해 만들어집니다. 그 라이브러리들은 어플리케이션을 만들기 위한 CLI 도구(예를 들면, create-react-app, @vue/cli)를 제공합니다. 이러한 CLI 도구는 대부분의 환경설정을 추상화하고 기본값을 제공합니다. 하지만 개발자로서의 당신은 이러한 기능이 얼마나 유익한지 이해하면, 기본값을 조정해 보고 싶어질 겁니다.

# 시작해보죠.

우리는 웹팩의 동작을 시험해보기 위해 간단한 어플리케이션을 만들겁니다. 처음에 새로운 폴더를 만들고 npm 프로젝트를 초기화 해주세요. 당신이 처음 폴더를 만든다고 가정하고 다음의 명령어를 입력해주세요.

```
npm init -y
```

이제 필수 웹팩 패키지를 설치합시다.

```
npm install --save-dev webpack webpack-cli webpack-dev-server
```

`package.json` 파일을 연 다음, 기존에 있던  `test` 스크립트를 지우고 새로운 스크립트 `dev` 를 추가해 개발 모드에서 웹팩이 실행되도록 합니다. 이 스크립트는 로컬에서 작업하기 편합니다. 패키지파일은 다음과 같이 보일 겁니다.

```jsx
{
  "name": "webpack-demo",
  "version": "1.0.0",
  "description": "To demonstrate the working of Webpack",
  "main": "index.js",
  "scripts": {
    "dev": "webpack --mode development"
  },
  "author": "Harsha Vardhan",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.44.1",
    "webpack-cli": "^3.3.12",
    "webpack-dev-server": "^3.11.0"
  }
}
```

이제  `npm run dev` 을 실행하면 ,다음과 같은 에러가 발생합니다. :  `Entry module not found`. 이건 기본적으로 웹팩이 entry point로서 `src/index.js` 를 찾기 때문에 발생합니다. 또한 웹팩은 `dist` 라는 폴더에 빌드한 파일들을 추출합니다.

이제 새로운 폴더 `src` 와 그 안에 `index.js` 파일을 만듭니다.

그리고 그 곳에 `console.log('hello');` 구문을 `index.js` 파일에 입력합니다.

그리고 `npm run dev` 을 실행하면 어떤 오류도 찾을 수 없고, `dist` 라는 폴더 안에 `main.js` 파일이 빌드 된 것을 확인할 수 있습니다.

# 웹팩 설정하기

웹팩을 설정하기 위해서 우리는 root 폴더에 `webpack.config.js` 라는 파일을 만들어야 합니다. 이 파일에서 우리는 환경설정 객체를 추출해야 합니다. 웹팩은 Node.js 같은 브라우저창이 없는 자바스크립트 환경에서 동작하기 때문이죠.

널리 사용되는 용어들은 다음과 같습니다. :

- **Entry point** — 모든 의존 객체들이 모인 웹팩의 시작점을 정의합니다. 이러한 의존들은 의존 그래프를 생성합니다.
- **Output** — 어느 곳에 빌드 과정에서 JS와 정적 파일들을 모을지 정의합니다.
- **Loaders** — 웹팩이 다양한 파일 확장자를 다룰 수 있도록 도와주는 서드파티 확장 프로그램들입니다. JS가 아닌 파일들을 모듈로 바꿔줍니다.
- **Plugins** — 웹팩의 동작 방식을 바꿔주는 서드파티 확장 프로그램들입니다.
- **Mode** — 개발(development) 과 생산(production) 두 모드를 정의합니다. 기본은 생산(Production)입니다.

이제 기본 엔트리 포인트와 추출할 폴더를 설정합니다.

## 엔트리 포인트 변경하기

만약 우리가 기본 `src` 폴더가 아닌 `source/index.js` 파일을 보기를 원하면 우리는 추출된 객체에 `entry` 속성을 추가하면 됩니다.

```jsx
module.exports = {
  entry: './source/index.js',
}
```

다음과 같은 방식으로도 사용할 수 있습니다.

```jsx
const path = require('path')

module.exports = {
  entry: { index: path.resolve(__dirname, 'source', 'index.js') },
}
```

## 추출 폴더 변경하기

기본 dist 폴더가 아닌 다른 폴더에 빌드 파일을 추출하고 싶다고 해보죠. 우리는 다음과 같이 output 속성을 사용해 세팅해줄 수 있습니다 :

```jsx
const path = require('path')

module.exports = {
  output: { path: path.resolve(__dirname, 'build'), filename: 'main.js' },
}
```

## 빌드 파일에 HTML 파일 포함하기

모든 웹 어플리케이션에는 최소한 하나의 HTML 파일이 존재합니다. HTML을 동작하게 하기 위해서 우리는 `html-webpack-plugin`이라고 불리는 플러그인을 사용합니다. 다음 명령어를 사용해 플러그인을 설치합니다.

```
npm install --save-dev html-webpack-plugin
```

## 플러그인은 정확히 어떤 일을 하는가 ?

플러그인은 HTML 파일을 읽고, 같은 파일에 번들을 삽입합니다.

이를 확인하기 위해 간단한 HTML 파일을 만들고 웹팩 환경설정을 합니다.

`index.html` 라는 이름의 파일을 `source` 폴더에 만들고, 몇몇 보일러 플레이트 코드를 삽입합니다. 이제 이 내용을 webpack 파일에 설정합니다.

```jsx
const HTMLWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
  plugins: [
    new HTMLWebpackPlugin({
      template: path.resolve(__dirname, 'source', 'index.html'),
    }),
  ],
}
```

파일이 준비가 됐으면, 이 파일을 받아줄 서버가 필요합니다. 그렇죠? 그래서 이전에 설치한 webpack-dev-server을 사용해 서버를 시작합니다.

## webpack **Dev Server**

`webpack-dev-server` 을 설정하기 위해 우리는 `package.json` 파일을 열고 서버를 시작하기 위한 새로운 스크립트를 추가합니다. 이미 파일을 빌드하기 위한 `dev` 라는 스크립트가 있으므로, 아래처럼 `start` 라는 스크립트를 만듭니다.

```jsx
"scripts": {
  "dev": "webpack --mode development",
  "start": "webpack-dev-server --mode development --open"
}
```

이제 터미널에서 아래 명령어를 실행합니다.

```
npm start
```

브라우저를 열어 [`localhost:8080`](http://localhost:8080) 을 확인합니다. index.html이 전달됐고, 개발자 도구를 열면 `main.js`가 번들링 되서 포함되어 있는 것을 확인할 수 있습니다.

# **webpack Loaders 사용하기**

앞서 언급했듯이, loader는 다른 파일 확장자를 다루기 위한 서드파티 확장 프로그램입니다.

[웹팩에서 사용할 수 있는 수많은 loader들이 있습니다.](https://webpack.js.org/loaders/)

이제 웹팩 환경 설정 파일에서 loader를 세팅해보겠습니다. loader는 이상한 문법을 갖고 있습니다. 우리는 `module` 이라는 키를 사용하는데 이는 loader의 배열인 `rules` 라 불리는 다른 속성을 구성하고 있습니다.

우리나 모듈로 취급하길 원하는 각각의 파일은 우리가 `rules` 배열에 객체로 추가해야 합니다. 모든 객체는 두 개의 속성으로 구성되어있습니다. 하나는 `test` 로 파일의 타입을 정의하고 다른 하나는 `use` 로 loader로 이루어진 배열입니다. `use` 에 정의된 loader는 오른쪽 부터 왼쪽으로 불러온다는 사실을 기억해두세요. loader를 정의할 때 순서는 중요합니다. loader를 사용하기 전에 문서를 참고하세요.

환경설정 파일에서 loader의 일반적인 구문은 다음과 같습니다:

```jsx
module.exports = {
  module: {
    rules: [
      {
        test: /\.filename1$/,
        use: ['loader-b', 'loader-a'],
      },
      {
        test: /\.filename2$/,
        use: ['loader-d', 'loader-c'],
      },
    ],
  },
}
```

# CSS 적용하기

웹팩에서 CSS를 적용하기 위해서 우리는 2가지 loader가 필요합니다. `css-loader` 와 `style-loader` 입니다.

다음 명령어로 설치합니다.

```
npm install --save-dev css-loader style-loader
```

새로운 파일 `source` 폴더에 `styles.css` 을 새롭게 만듭니다. 서버에 실행시켰을 때 index.html에 보여줄 기본적인 css를 작성합니다.그 다음에 우리는 css 파일을 `index.html` 이 아닌 `index.js` 에 import 해줍니다. 이 튜토리얼은 웹팩과 번들링을 배우는데 기반을 둔다는 것을 기억하세요. 이제 `index.js` 파일은 다음과 같습니다.

```jsx
import './styles.css'
console.log('Im from source!')
```

한 가지 더 작업할 것이 있습니다. `webpack.config.js` 에 loader를 설정하는 것입니다. 설정한 후의 모습은 다음과 같습니다.

```jsx
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
}
```

`css-loader` 는 css 파일을 불러오는데 사용되고 `style-loader` 는 DOM에서 스타일 시트를 불러오는데 사용됩니다.

서버를 다시 시작하고 `npm start` 를 실행시키면 브라우저에 반영된 것을 볼 수 있습니다.

# **SASS 적용하기**

SASS(.scss) 파일을 적용하기 위해서는 세가지 loader를 필요로 합니다. `sass-loader` , `css-loader` , `styled-loader`. 또한 Node에서 필요로 하는 `sass` 패키지가 필요합니다. `sass-loader` 는 SASS 파일을 import해 불러오기 위해 사용됩니다. 다른 2가지는 이미 설치했기 때문에 남은 패키지들만 설치하겠습니다.

```
npm install --save-dev sass-loader sass
```

`source` 폴더 안에 새로운 파일 `styles.scss` 를 만들고 다음과 같은 기본 스타일을 추가합니다.

```sass
$primary-bg: aqua;

body {
    background-color: $primary-bg;
}
```

`import './styles.scss'` 을 사용해 `index.js` 파일 최상단에 이 파일을 include합니다.

이제 `webpack.config.js` 파일을 추가할 시간입니다. loader는 다음과 같습니다.

```jsx
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
      {
        test: /\.scss$/,
        use: ['style-loader', 'css-loader', 'sass-loader'],
      },
    ],
  },
}
```

서버를 재시작해서 변화를 확인합니다.

## 모던 자바스크립트 적용하기

웹팩 자체로는 모던 자바크스립트를 모든 브라우저에서 호환되는 코드로 바꿀수가 없습니다. 그래서 이를 위해선 Babel 을 사용합니다. 이를 위해서 먼저 `@babel/core` 라는 실제 엔진 패키지를 설치하고, 웹팩에서 필요로하는 loader `babel-loader` 와 자바스크립트를 ES5로 변환시켜주는 `@babel/preset-env` 를 설치합니다.

```jsx
npm install --save-dev @babel/core babel-loader @babel/preset-env
```

다음 단계는 root 폴더에 babel.config.json이라는 이름의 파일을 만들어 babel을 설정하는 것입니다. 우리가 설치한 preset-envm을 다음과 같이 설정해줍니다.

```jsx
{
    "presets": ["@babel/preset-env"]
}
```

이제 webpack.config.js 파일에 설치한 loader를 추가합니다.

```jsx
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
      {
        test: /\.scss$/,
        use: ['style-loader', 'css-loader', 'sass-loader'],
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: ['babel-loader'],
      },
    ],
  },
}
```

이제 JS 코드에 ES6 구문을 적용하고, 다시 빌드해보면 main.js 파일에 자동적으로 브라우저에 호환되는 변환 코드가 빌드 된 것을 확인할 수 있습니다.

## 웹팩에서 사용되는 모드들

웹팩서 사용되는 모드는 2개의 종류가 있습니다. 개발(development)과 생산(production).

**개발 모드**에서는 더 줄이는 작업이 없습니다. \*\*\*\*웹팩은 단순히 모든 JS 코드를 쓰고 브라우저에서 읽어서 어플리케이션을 빠르게 reload합니다.

**생산 모드** 에서는 웹팩은 많은 최적화를 적용합니다. `terser-webpack-plugin` 을 통해 자동으로 최적화를 적용하고 번들 사이즈를 줄입니다. 또한 `production` 을 위해 `process.env.NODE_ENV` 같은 유용한 환경 변수 설정을 통해 퍼포먼스를 향상시킵니다.

생산모드에서 웹팩을 사용하기 위해 `package.json` 에 다른 스크립트를 추가합니다. 이름을 `build` 라 하겠습니다.

```jsx
"scripts": {
    "dev": "webpack --mode development",
    "start": "webpack-dev-server --mode development --open",
    "build": "webpack --mode production"
  },
```

# 최적화 - 코드 분리

코드 분리(Code splitting) 혹은 게으른 로딩(lazy loading)은 큰 번들을 피하기 위한 최적화 기법입니다. 또한 의존의 중복 역시 피합니다. 이 메커니즘을 사용해 가령, 사용자가 버튼을 클릭하거나, 라우트가 변경되거나 등등.의 동작을 할 때 원하는 코드의 조각을 불러옵니다. 이 코드 조각은 덩어리(_chunk)_ 라는 이름으로 부립니다.

웹팩에는 어플리케이션의 초기 번들 크기는 244kb이하로 해야된다는 제한이 있습니다. 웹팩에서 코드 분리를 하기 위한 방법에는 3 가지가 있습니다. :

1. 여러개의 엔트리 포인트를 갖는 것
2. `optimization.splitChunks` 사용
3. 동적 import

첫 번째 방법은 작은 프로젝트에서 잘 작동하지만 복잡하거나 큰 프로젝트에서는 그렇지 않습니다. 웹팩 환경 설정 파일에서 여러개의 엔트리 포인트를 정의합니다.

## **optimization.splitChunks 사용**

어플리케이션에서 많은 의존을 사용하게 되는 때가 있습니다. 예를 들어, 날짜를 위한 유명한 패키지 `Moment` 를 사용해봅시다. 저는 이것이 상대적으로 가벼운 패키지이기 때문에 선택했습니다. 설치하고 index.js 파일에 include한 다음 build 명령어를 실행합니다.

```jsx
npm install moment
```

Moment가 성공적으로 설치됐으면 index.js 파일에 import 합니다.

```jsx
npm run build
```

![https://miro.medium.com/max/4800/1*hcOcj2SVpEXJuwhOyZdBXQ.png](https://miro.medium.com/max/4800/1*hcOcj2SVpEXJuwhOyZdBXQ.png)

이걸 어떻게 해결할까요? 간단합니다. 다음과 같이 optimization이라는 key 이름을 추가하고 splitChunks 라는 다른 속성을 추가합니다.

```jsx
module.exports = {
  optimization: {
    splitChunks: { chunks: 'all' },
  },
}
```

엔트리 포인트가 상당히 줄어든 것을 확인할 수 있습니다.

## 동적 **imports**

동적 imports는 조건에 따라 코드를 불러옵니다. 이러한 접근법은 React 와 Vue에서 널리 사용됩니다. 우리는 사용자 인터랙션에 기반해 코드를 불러오거나 route 변경에 따라 불러옵니다.

시연을 위해 우리 페이지에 버튼을 추가하겠습니다. 이 버튼은 클릭하면 포스트 목록을 전달합니다. 목록을 가져오는 로직은 현재와는 분리된 파일에 위치합니다. 그 코드는 동적으로 index.js 파일에 import 됩니다.

`source` 폴더에 `api.js` 라는 파일을 만들어 `fetch` API를 호출합니다. 여기서 우리는 공용 API로 request을 만들고 response를 반환하는 함수를 export합니다.

```jsx
export const fetchTodos = () => {
  return fetch('https://jsonplaceholder.typicode.com/posts')
    .then(response => response.json())
    .then(json => json)
}
```

`index.html` 파일에 버튼을 만들어 `id` fmf `btn` 이라고 합니다.

```html
<button id="btn">Click me to load</button>
```

`index.js` 파일 안에는 `api.js` 파일을 import 하기 위해 동적 importing을 합니다.

```jsx
const getTodos = () => import('./api')
```

또한, 파일 끝부분에 다음 코드를 추가해 가져온 데이터를 로그로 보여주는 로직을 추가합니다.

```jsx
const btn = document.getElementById('btn')
btn.addEventListener('click', () => {
  getTodos().then(({ fetchTodos }) => {
    fetchTodos().then(resp => console.log(resp))
  })
})
```

이 `getTodos` 라 불리는 코드 조각을 통해 파일을 import 해옵니다. 그리고 import 후에 fetchTodos 속성을 비구조화 하고 함수를 호출하여 성공에 대한 response를 로그에 보여주는 역할을 합니다.

이제 파일들을 빌드하고 서버를 가동하여 개발자 도구를 열고 네트워크 탭을 엽니다.

클릭해보면 새로운 JS 파일이 동적으로 불러와졌음을 확인할 수 있습니다.
