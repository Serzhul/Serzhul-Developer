---
title: Redux 개념 및 작성 예시
date: 2021-09-23 15:09:75
category: React
thumbnail: { thumbnailSrc }
draft: false
---

# Redux란 무엇인가?

- 한마디로 요약하면 컴포넌트간 혹은 app 전역 상태간 상태 관리 시스템이라 할 수 있다.

## State

State의 종류에는 일반적으로 3가지가 있다.

- Local State
- Cross-Component State
- App-Wide State

### 지역 State(Local state)

- 하나의 컴포넌트에 종속되는 state를 의미한다.
- 예를 들면, input 필드에서 user가 입력한 input 값을 받는 것을 들 수 있다.
- 지역 상태는 컴포넌트 내붕[서 useState() 혹은 useReducer()를 통해 관리된다.

### 컴포넌트간 State(Cross-Component State)

- 여러개의 컴포넌트에 영향을 미치는 state를 말한다.
- 예를 들면 모달의 오버레이를 열고 닫는 것을 예로 들 수 있다. (모달은 다양한 외부 컴포넌트에서 접근 가능하므로)
- prop chains 혹은 prop drilling을 필요로 한다.

> **prop drilling(prop chain)이란?**

- React에서 전역 상태를 관리할 때 하위 component에 props를 넘겨 줘야할 때 직접 전달할 수 없으므로 component간에 props를 넘겨줘서 최하위 컴포넌트까지 전달되도록 하는 동작 방식을 말한다.
- 하위에서 하위 컴포넌트로 props가 이동되는 과정을 드릴로 땅을 파는것에 비유해 drilling이라고 부르고 국내에선 프로퍼티 내리꽂기로 번역되었다. ([https://edykim.com/ko/post/prop-drilling/](https://edykim.com/ko/post/prop-drilling/) 글 참조)

### 앱-전역 State(App-Wide State)

- 어플리케이션 전체에 영향을 주는 State를 말한다. (거의 혹은 모든 컴포넌트에)
- 예를 들어 user authentication status 같은 정보를 들 수 있다.
- Cross-Component와 마찬가지로 prop chains(prop drilling)을 필요로 한다.

컴포넌트간 상태나 앱-전역 State, 특히 앱-전역 State를 관리할 때는 React Context 혹은 Redux가 도움을 줄 수 있다.

# Redux vs React Context

- 이미 React에서는 자체적으로 'React Context'를 제공해 state를 관리할 수 있도록 되어있다.
- 그럼에도 불구하고 Redux가 사용되는 이유는 React Context(Context API)가 갖고있는 잠재적 단점 때문이 있기 때문이다.

## React Context의 잠재적 단점들

- 복잡한 세팅 및 관리
  - 큰 규모의 어플리케이션을 관리할 때, 크고 다른 Context Provider들을 필요로 한다.
  - 좀 더 복잡한 어플리케이션에서 React Context를 관리하는 것은 깊게 중첩된 JSX 코드를 만들거나 매우 큰 규모의 Context Provider 컴포넌트를 만들게 된다.
- 성능
  - React Context는 모든 경우와 시나리오에 있어서 Redux의 훌륭한 대체재가 되지 못한다.
  - React Context는 state가 높은 빈도로 자주 변하는 경우에 최적화되어 있지 않다.

# Redux는 어떻게 작동하는가?

## Redux의 핵심 컨셉

- Redux는 어플리케이션에서 `하나의 중앙 데이터(state) 저장소(store)`를 갖는다.
- 컴포넌트 내부에서 데이터를 사용할 수 있다는 것은, 중앙 데이터 저장소를 `구독(subscription)`한다고 표현한다.
- 컴포넌트가 하는 역할의 비중이 낮아진다.(대부분 상태 관리를 하지 않으므로) 또한, 저장소 데이터를 직접적으로 조작할 수 없다.
- 또 설정해줘야 하는 것으로 Reducer 함수가 있다. 이 함수는 저장소 데이터가 변할 수 있도록 하는 역할을 맡는다.
  - 여기서 말하는 reducer는 react의 useReducer()하고는 다르다. Reducer 함수는 일반적인 컨셉이다.
- 컴포넌트는 상태를 변경시키기 위해 특정 action을 발생시키는데, 이를 Dispatch라고 하며 Reducer에 dispatch가 전달된다.
- 여기서 action이란, reducer가 동작해야 하는 절차를 표현하는 JavaScript 객체를 의미한다. Redux는 action 객체를 reducer에 전달하고 reducer는 요청된 절차를 읽어서 수행하게 된다.

## Reducer 함수(The Reducer Function)

- Reducer 함수는 순수함수여야 한다.
  - 같은 input값을 넣으면 같은 output값이 반환되야한다.
- Input 인자로는 OldState값과, Dispatch된 Action 객체를 받는다.
- Output으로는 새로운 State 객체를 반환한다.

# Redux 작성 예시

### 1. reducer

```jsx
const counterReducer = (
  state = {
    counter: 0,
  },
  action
) => {
  switch (action.type) {
    case 'ADD':
      return {
        counter: state.counter + 1,
      }
    default:
      return state
  }
}
```

### 2. store

```jsx
const redux = require('redux')

const store = redux.createStore(counterReducer)
```

### 3. subscriber

```jsx
const counterSubscriber = () => {
  const latestState = store.getState() // store의 최신 state를 가져옴
  console.log(latestState)
}

store.subscribe(counterSubscriber) // 상태 값이 변경될 때마다 counterSubscriber가 실행됨
```

### 4. trigger dispatch

```jsx
store.dispatch({ type: 'ADD' }) // console => {counter : 1}
```

- JavaScript에서 Redux를 사용하는 간단한 에제를 만들었다. 이처럼 Redux는 Vanilla JS에서도 사용 가능하지만 주로 React와 같이 사용하는 경우가 많은데, 아래에선 React에서의 작성 예제를 살펴보겠다. (좀 더 추가되는 단계가 있다.)

# React에서 Redux 사용하기

## 1. reducer

```jsx
/* store/index.js */

const initialState = {
  counter: 0,
}

const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'ADD':
      return {
        ...state,
        counter: state.counter + 1,
      }
    default:
      return state
  }
}
```

## 2. store

```jsx
import { createStore } from 'redux'

const store = createStore(counterReducer)

export default store
```

## 3. Provider (전역에서 store 사용가능하도록 전달)

```jsx
/* App.js */

import Counter from './components/Counter'
import { Provider } from 'react-redux'
import store from './store'

function App() {
  return (
    <Provider store={store}>
      ;
      <Counter />
    </Provider>
  )
}

export default App
```

## 4. useSelector

- useSelector()는 Provider로 감싼 store에서 state 데이터를 가져올 수 있게 해준다.
- useSelector를 사용하면 react-redux는 자동적으로 해당 컴포넌트가 store를 subscription 하도록 도와준다. 즉, 상태 값이 변경되면 업데이트 하고 re-rendering 할 수 있도록 도와준다는 것이다.

```jsx
import { useSelector } from 'react-reudx'

const Counter = () => {
  const counter = useSelector(state => state.counter)
}
```

## 5. dispatch

- useDispatch를 사용해 dispatch 함수를 만들어줄 수 있다.
- dispatch에 액션 객체를 전달하는 방식으로 dispatch를 작동시킨다.
- 액션 객체는 type 뿐만 아니라 전달하고 싶은 속성값이 있다면 추가할 수 있다.

```jsx
const dispatch = useDispatch()

const counterHandler = () => {
  dispatch({ type: 'ADD' })
}
```

## 6. 최종 작성 예시

```jsx
import React from 'react'
import { useSelector, useDispatch } from 'react-redux'

export default function Counter() {
  const counter = useSelector(state => state.counter)

  const dispatch = useDispatch()

  const counterHandler = () => {
    dispatch({ type: 'ADD' })
  }

  return (
    <div>
      <div>{counter}</div>
      <button onClick={counterHandler}>+</button>
    </div>
  )
}
```
