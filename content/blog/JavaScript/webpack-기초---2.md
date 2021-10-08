---
title: Webpack 기초 - 2
date: 2021-10-08 10:10:27
category: JavaScript
thumbnail: { thumbnailSrc }
draft: false
---

# Bundle이란?

- 모듈은 JavaScript 뿐 아니라, 다른 여러 포맷들의 파일도 모듈화가 가능하다.
- 이렇게 참조 관계가 있는 여러 파일들을 하나로 합쳐주는 것을 Bundling이라고 한다.

# Bundle이 중요한 이유

- 모든 모듈을 로드하기 위해 검색하는 시간이 단축된다.
  - Bundle이 되어있지 않을 경우 여러 참조관계에 의해 파일에 접근하고 접근을 끝는 과정이 반복되는 과정에서 리소스가 낭비됨.
  - 하지만 Bundling된 파일들은 모두 하나의 파일에 모듈이 담겨져 있으므로, 이런 낭비를 줄일 수 있음
- 사용하지 않는 코드를 제거해줌
  - Bundling 이후 참조하는 내용을 제외한 나머지 코드들은 제거됨.
  - 결과적으로 파일의 크기도 줄어들고 퍼포먼스도 향상됨.
- 파일의 크기를 줄여든다.

# Webpack이란?

Web application을 위해 사용하는 모듈 번들러

- Webpack을 사용하는 이유가 무엇인지, Webpack을 사용하면 어떤 부분에서 도움이 되는지를 이해해야함

# Webpack 기본 구조

- 서로 의존성을 가지고 있는 Module들
  - js : JavaScript 파일
  - sass : Sass 파일
  - hbs : Handlebars 파일
    - Handlebars는 템플릿 엔진의 일종으로, 데이터를 특정한 양식에 따라 출력해주는 도구이다.
  - jpg, png : image 파일
- 여러 파일들을 확장자로 묶어서 Bundling 함

## 1. Entry & Output

### Entry

모듈의 의존 관계를 이해하기 위한 시작점을 설정

- 가령 Moudle A가 Module B와 C를 참조하고 있다고 하면, Module A는 Moudle B,C에 의존 관계를 갖고 있다고 할 수 있다. Webpack은 이런 모듈들의 의존 관계를 참조해 의존성 그래프를 만들고 번들링을 함
- Entry는 참조 관계의 가장 상위에 있는 모듈로 작성해줘야함 (위의 경우 Module A가 Entry가 됨)

### Output

Webpack이 생성하는 번들 파일에 대한 정보를 설정

## Webpack 설정

Webpack 설치

```bash
$ npm install webpack-cli --save-dev
```

- 설치후 npx 명령어를 통해 webpack을 실행할 수 있지만, 초기에는 몇가지 설정이 필요하다.
- 앞서 언급했던 것 처럼 Entry와 Output에대한 정보가 누락되었기 때문.
- 하지만 webpack 4버전 이후에는 zero configuration(기본 자동 설정해주는 기능)이 업데이트 되어 쉽게 세팅할 수 있다.
- 단, zero configuration을 위해서는 entry 설정을 위해 src라는 이름의 폴더에 index.js라는 파일과 나머지 모듈 파일들이 있어야하고 bundle 된 파일도 main.js라는 이름으로 dist라는 폴더에 위치해야 하게 되므로 폴더가 있어야한다.

### 설정시 주의 점

- readline을 사용시 Node.js의 내장모듈이라고 webpack이 인식하지 못해 에러가 발생할 수 있다.
  - Node.js 환경에서 실행되고 있음을 인식시켜주기 위해 target이라는 키 설정해줘야함.
  - target은 webpack에게 어떤 환경에서 실행되는지 알려주는 역할을 함.
  ### 명령어
  ```bash
  $ npx webpack --target=node
  ```

## webpack.config.js 작성하기

- webpack 환경설정을 위해 해당 파일을 프로젝트 폴더에 작성한다.

### webpack.config.js 작성 예시

```jsx
const path = require('path')

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'), // 절대경로로 설정해야 함
    filename: 'bundle.js',
  },
  target: 'node',
}
```

## 2. Mode

- Package.json —save-dev, —save
  - 외부에서 받은 모듈을 2가지로 나누고 있음
    - 어플리케이션 내부에 직접 포함되는 모듈 (e.g Jquery) : dependecies 키로 저장
    - 개발 과정에 필요한 모듈 : devDependencies로 저장
    - —save는 dependecies에 기록, —save-dev는 devDependencies에 기록되어야한다고 말하는 것과 같다.
  - 위와 같은 과정을 진행하면 다른 환경에서 프로젝트를 불러올 때 의존성 기록에 의거하여 필요한 모듈들을 다운로드 받을 수 있다.
- 개발환경과 프로덕션 환경
- Mode & Webpack-merge

## 3. Loader

Loader는 Webpack이 의존성 그래프를 완성시키는 과정에서 다양한 모듈들을 입력받아 처리하는 역할을 한다.

- Webpack이 기본적으로 인식하는 파일은 .js 파일 또는 .json 파일이므로 다른 타입 모듈은 개별적으로 Loader를 준비해 webpack에 연결시켜줘야 함.

```jsx
module.exports = {
  module: {
    rules: [loader1, loader2],
  },
}
```

- 위 코드에서 rules라는 키는 지원해야 하는 타입들의 loader들을 설정하는 공간이다.
- 배열 타입으로 loader들의 값을 받게 되는데, loader에 대한 정보를 넣을 때는 loader내에 설정된 기본동작이 적용되도록 loader의 이름을 `**문자열**`로 넣는 방법과, 어떻게 동작할 지 자세하게 설정할 수 있는 `**객체 형태**`로 넣는 방법이 있다.

e.g)

css loader : css를 모듈로 다루기 위해 사용되는 loader

style loader : 가져온 css를 style 태그를 생성해 html에 삽입하도록 해주는 loader

## 브라우저 환경(web)에서 webpack 적용하기

target의 기본값은 web으로, 설정해주지 않으면 기본값이 적용된다.

### webpack.config.js 파일 작성 예시

```jsx
const path = require('path')

module.exports = {
  entry: './index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
          },
        ],
      },
    ],
  },
  mode: 'none',
}
```

### cf)

- 스타일 작업전에 브라우저별로 적용되는 user agent style을 통일하기 위해 사용
- 스타일 작업시 개별 우선 적용되는 스타일이 브라우저 마다 다르게 존재함.
- user agent를 평준화 하는 방법은 2가지가 있음
  - reset.css ⇒ 주로 적용된 스타일을 제거
  - normalize.css ⇒ 주로 적용된 스타일 중 다른 부분을 같게 동작하도록 만들어줌

## 4. Plugin

Plugin은 Webpack이 동작하는 과정에서 개입하여 여러가지 작업을 행할 수 있음

- bundle 파일에 변화를 주기도 한다.
- 개발모드에서 개발 편의성을 제공
- 프로덕션 모드에서 코드의 최적화를 지원 등

```jsx
module.exports = {
	plugins: [new Plugin({...option}, ...]
}
```

# 요약

## Webpack의 구조 5가지

### Entry

- Webpack은 복잡한 모듈의 상관관계를 분석하여 의존성 그래프를 만드는데 그 시작점을 설정하는 것이 Entry이다.

### Output

- Entry를 기점으로 의존성 그래프를 만들고, bundle이 끝나면 output이라는 요소에 설정된 정보를 기반으로 bundle된 파일이 생성된다.

### Mode

- Mode 키를 통해 빌드 환경을 구분지을 수 있음
  - 개발 모드
  - 프로덕션 모드

### Loader

- Loader는 모듈을 입력받아 처리하는 것과 과정된 요소이다.
- webpack은 모듈을 해석할 때 JS 파일이나, JSON파일을 기본으로 해석하므로 그 외의 파일에 대해서는 loader 설정을 따로 해줘야 모듈로 불러와서 번들링을 할 수 있음.
- test라는 키를 통해 모듈을 인식하기 위한 패턴을 지정
- use라는 키를 통해 사용할 loader와 옵션을 설정해서 모듈을 처리할 때 어떻게 처리할 지 구체적으로 정할 수 있음

### Plugin

- Plugin을 설정하면 번들링의 여러 과정 안에서 작업을 할 수 있음.
- Plugin 안에 Plugin 객체를 할당하면 Plugin들을 활용할 수 있음.
