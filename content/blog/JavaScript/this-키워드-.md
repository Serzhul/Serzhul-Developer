---
title: This 키워드
date: 2021-04-10 21:04:90
category: JavaScript
thumbnail: { thumbnailSrc }
draft: false
---

# This 키워드(변수)

- 모든 실행 컨텍스트(모든 함수)가 만들어질 때마다 생성되는 특별한 변수.
- this 키워드가 사용된 함수의 주인을 가리키는 값으로 사용된다.
- this 키워드는 `고정적이지 않다`. 함수가 어떻게 호출되는지에 따라 달라지는데, 함수가 실제로 호출 될때에만 값이 할당된다.
- this 키워드는 함수 자체를 가리키는 것도 아니며, 함수 환경(variable environment)를 가리키는 것도 아니다.

# 함수 호출 방식에 따른 This 키워드의 변화

### 메서드(Method)

- Method에서의 this는 `메서드를 호출하고 있는 객체 값` 을 의미한다.

```jsx
const serzhul = {
  name: 'Serzhul',
  year: 1990,
  calcAge: function() {
    return 2037 - this.year
  },
}

serzhul.calcAge() // 47
```

- 위 코드에서 calcAge라는 메서드의 내부에서 this 키워드를 갖고 있는데, calcAge의 메서드를 호출하고 있는 객체는 serzhul이므로, this.year에서 this는 serzhul 객체를 가리킨다. 따라서 this.year에는 1990이 할당되게 된다.

### 단순 함수 호출(Simple function call)

- 단순 함수를 통해 호출하게 되면, this 키워드는 undefined값이 할당된다. (Strict 모드에 한해서)
- Strict 모드가 아닐 경우 this 값에는 전역 객체인 window 객체의 값이 할당되는데, 그 경우에는 this가 적절하게 사용되지 않는 문제가 발생하므로 Strict모드의 사용이 권장된다.

```jsx
'use strict'

const serzhul = {
  firstName: 'Serzhul',
  year: 1990,
  calcAge: function() {
    const isOlderThan30 = function() {
      console.log(this) //
      console.log(this.year >= 1990)
    }

    isOlderThan30()
  },
}

serzhul.calcAge() // undefined
// Uncaught TypeError
```

- 위 예시에서 calcAge 함수 내의 isOlderThan30 함수는 this 키워드를 로그에 출력하고, calcAge 함수 내부에서 호출된다. 이 때 단순함수를 통해 호출하게 되면 this 키워드는 undefined값이 할당되므로, undefined가 출력되며, undefined에 year라는 프로퍼티는 존재하지 않으므로 에러가 발생한다.

위와 같은 방법을 해결하기 위해 다음과 같이 코드를 수정할 수 있다.

### Solution1

```jsx
'use strict'

const serzhul = {
  firstName: 'Serzhul',
  year: 1990,
  calcAge: function() {
    const self = this
    const isOlderThan30 = function() {
      console.log(self) //
      console.log(self.year >= 1990)
    }

    isOlderThan30()
  },
}

serzhul.calcAge() // {firstName: "Serzhul", year: 1990, calcAge: ƒ}
// True
```

### Solution2

```jsx
'use strict'

const serzhul = {
  firstName: 'Serzhul',
  year: 1990,
  calcAge: function() {
    const isOlderThan30 = () => {
      console.log(this) //
      console.log(this.year >= 1990)
    }

    isOlderThan30()
  },
}

serzhul.calcAge() // {firstName: "Serzhul", year: 1990, calcAge: ƒ}
// True
```

### 화살표 함수(Arrow functions)

- 화살표 함수에서의 this 키워드는 함수 자체의 this 키워드를 가질 수 없다. 따라서, 화살표 함수를 감싸고 있는 함수(부모 함수)의 this 키워드의 값을 갖게된다.
- 따라서 화살표 함수에서의 this 키워드는 외부 lexical scope에서 this keyword 값을 찾아 할당받게 된다.

```jsx
const serzhul = {
  firstName: 'Serzhul',
  year: 1990,
  calcAge: function() {
    return 2037 - this.year
  },

  greet: () => console.log(`Hi, I'm ${this.firstName}`),
}

serzhul.greet() // 'Hi, I'm undefined'
```

- 위 예시에서 greet를 호출해 출력한 this의 name은 undefined가 되는데 그 이유는, 화살표 함수는 자체의 this 키워드를 가질 수 없고, 화살표 함수의 부모의 스코프의 값을 갖게 된다.
- 위 예시의 경우, greet 함수의 부모는 전역 스코프로, 전역스코프에서의 this는 window 객체이지만, window 객체에 firstName이라는 속성은 없으므로 undefined로 값이 할당된다.
- 위와 같은 원리로 var 변수를 선언할 때 주의해야 하는 점이 생기는데, 바로 var로 선언을 하게 되면 새로운 프로퍼티가 window 객체에 추가되기 때문이다.

```jsx
var firstName = 'serzhul'

const serzhul = {
  firstName: 'Serzhul',
  year: 1990,
  calcAge: function() {
    return 2037 - this.year
  },

  greet: () => console.log(`Hi, I'm ${this.firstName}`),
}

serzhul.greet() // 'Hi, I'm serzhul'
```

- 따라서 method로 사용할 함수로 화살표형 함수를 선언하는 것과, var로 변수를 선언하는 것은 바람직하지 않다고 이해할 수 있다.

### 이벤트 리스너(Event listener)

- 이벤트 리스너에서 this 키워드 값은 항상 핸들러로 추가된 DOM 요소 값을 가리킨다.
