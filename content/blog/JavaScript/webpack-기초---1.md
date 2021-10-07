---
title: Webpack 기초 - 1
date: 2021-10-07 12:10:90
category: JavaScript
thumbnail: { thumbnailSrc }
draft: false
---

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

# Module

## Module이란?

프로그램을 구성하는 내부의 코드가 기능별로 나누어져 있는 형태

## Module의 표준

- Module을 사용하기 위해서는 Module을 인식하는 Module System과 키워드가 필요하다.
- Moudle의 표준은 크게 2가지
  - CommonJS(Node.js)
  - ESM (ECMAScript 2015 ~)
- Module을 다루는 키워드
  - 내보내기
    - 한 곳으로 내보내기
    - 개별적으로 내보내기
  - 가져오기
    - 모듈 객체를 참조하는 형태

### CommonJS

- 모듈 가져오기
  - require 키워드 사용
  ```jsx
  require(모듈 경로)
  ```
- 모듈 내보내기
  - module.exports 사용
  ```jsx
  module.exports = {...}

  module.exports = 값

  module.exports.키_이름 = 값

  exports.키_이름 = 값
  ```

### 코드 예시

```jsx
// 모듈 내보내기1

const PI = 3.14
const calCircle = r => r * r * PI

module.exports = {
  PI,
  calCircle,
}

// 모듈 내보내기2

exports.PI = PI
exports.calCircle = calCircle
```

```jsx
// 모듈 가져오기
const { calCircle } = require('./mathUtil')

const result = calCircle(2)
console.log(result)
```

### ESM

- 모듈 가져오기
  - import 키워드 사용
  ```jsx
  import 모듈_이름 from 모듈_위치
  ```
- 모듈 내보내기
  - export, export default 키워드 사용
  - 둘의 차이는 export는 각각의 요소의 이름을 그대로 가져다 사용한다고 하면 export default는 그 요소들을 묶어주는 하나의 공통된 요소의 이름으로 사용한다.
  ```jsx
  export 모듈_이름
  export default 모듈_이름
  ```

### ESM 방식으로 모듈 내보내고 가져오는 코드 예시

```jsx
// 모듈 내보내기
const PI = 3.14
const calCircle = r => r * r * PI

export { PI, calCircle }
```

```jsx
// 모듈 가져오기

import { calCircle } from './mathUtil'

const result = calCircle(2)
console.log(result)
```

# Module의 종류

- Built-in Core Module (e.g Node.js module)
- Community-based Module (e.g NPM)
  - npm CLI를 통해 접근 가능 (e.g npm install ModuleName)
- Local Module (특정 프로젝트에 정의된 모듈)

### readline

```jsx
const readline = require('readline')
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
})

rl.question('원하는 도형을 작성해주세요 : ', input => {
  console.log(input)
  rl.close()
})
```

- question이라는 메서드는 사용자로부터 입력을 받기 전에 입력과 관련된 정보를 전달하고, 입력된 데이터를 콜백함수로 다룰 수 있게 함
- 첫 번째 파라미터는 입력 정보를 제공해주는 문자열, 두 번째 파라미터는 사용자 입력이 발생하면 실행되는 콜백 함수 (사용자가 입력한 데이터를 인자로 전달받음)

### Module 응용 예시

```jsx
const readline = require('readline')
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
})

const { calSquare, calCircle } = require('./mathUtil')
const { logInput, logResult, logFigureError } = require('./log')

rl.question('원하는 도형을 작성해주세요. (정사각형, 원 ) : ', figure => {
  console.log(`선택된 도형: ${figure}`)

  switch (figure) {
    case '정사각형':
      rl.question('변의 길이를 입력해주세요 : ', input => {
        console.log(logInput(input))
        console.log(logResult(figure, calSquare(input)))
        rl.close()
      })
      break
    case '원':
      rl.question('반지름의 길이를 입력해주세요 : ', input => {
        console.log(logInput(input))
        console.log(logResult(figure, calCircle(input)))
        rl.close()
      })
      break
    default:
      console.log(logFigureError)
      rl.close()
  }
})
```

```jsx
const logInput = input => `입력받은 값 : ${input}`
const logResult = (figure, result) => `${figure}의 넓이는 : ${result} 입니다.`
const logFigureError =
  "지원되지 않는 도형입니다.  '정사각형' 또는 '원'을 입력해주세요. \n 커맨드 라인을 종료합니다."

module.exports = {
  logInput,
  logResult,
  logFigureError,
}
```

## Module 사용시 알아둬야 할 것

- 코드의 재사용성이 증가한다.
- 코드의 관리가 편해진다.
- 코드를 모듈화 하는 기준이 명확해야 한다.
