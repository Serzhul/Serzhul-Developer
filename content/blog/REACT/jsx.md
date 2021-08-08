---
title: JSX
date: 2021-08-08 18:08:23
category: REACT
thumbnail: { thumbnailSrc }
draft: false
---

# JSX

- JSX에서는 단 하나의 Wrapper 태그가 있어야 하며, 그 안에 컴포넌트를 삽입하는 형태로 구성되어야 한다. 2개 이상의 태그를 사용하려고 하면 오류가 발생한다.
- JSX 문법에서 독특한 점은 바로 props라 할 수 있다. html 태그에서 attribute를 적용했듯이, JSX에서는 props라 불리는 속성들을 정의할 수 있는데 사용자가 정의한 이름과 값을 각 컴포넌트에서 key와 value 값으로 불러 사용할 수 있다.

Ex)

```jsx
// App component의 구조
import React, { Component } from 'react'
import Person from './Person/Person'
import './App.css'

class App extends Component {
  render() {
    return (
      <div className="App">
        <Person name="Serzhul" age="26">
          Greetings!
        </Person>
        <Person name="Sapphire" age="22" />
        <Person name="Momo" age="21" />
      </div>
    )
  }
}

export default App

// Person Component의 구조
import React from 'react'

const person = props => {
  return (
    <div>
      <p>
        I'm {props.name} and I am {props.age} years old!
      </p>
      <p>{props.children}</p>
    </div>
  )
}

export default person
```

- 위 예시 코드를 살펴보면 App 컴포넌트 내에서 Person 컴포넌트를 3번 호출하는데, 각각의 props를 name과 age를 정의하여 전달하고 있다. 이 props를 전달받아 person 컴포넌트에서는 name과 age라는 키값을 사용할 수 있게 되고 `{}` 을 사용하여 props 값을 출력하도록 할 수 있다.
- props는 사용자 정의 속성 외에도 children이라는 속성을 갖고있는데 이는 컴포넌트가 호출될 때 자식요소의 내용 전부를 갖고 있는 속성이라 할 수 있다. 위의 예시에서는 Greetings!라고 되어있는 텍스트가 자식요소 이므로 첫번째 Person 컴포넌트를 호출할 때에는 Greeting라는 텍스트가 추가로 나타나는 것을 볼 수 있다.
- 이렇게 컴포넌트를 랜더링할 때 외부에서 props를 전달할 수 있지만, 내부에서 상태를 제어하기 위해 state를 사용할 수도 있다.
- state 역시 props처럼 객체 형태로 key,value를 지정해 사용할 수 있으며, props처럼 값을 할당할때는 this.state 형태로 사용한다.
- 이 state는 class형태의 컴포넌트에서만 사용할 수 있으며, state의 값이 바뀔 때마다 컴포넌트는 리랜더링을 통해 스테이트를 업데이트한다.
- props가 외부에서 여러개의 컴포넌트를 통해 전달될 수 있다면, state는 하나의 동일한 컴포넌트 내에서 적용된다고 볼 수 있다.
- props는 변수는 물론이고 함수도 전달할 수 있다.
