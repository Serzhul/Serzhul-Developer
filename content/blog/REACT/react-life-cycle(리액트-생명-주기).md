---
title: React life cycle(리액트 생명 주기)
date: 2021-09-19 16:09:36
category: React
thumbnail: { thumbnailSrc }
draft: false
---

출처 : [https://www.w3schools.com/react/react_lifecycle.asp](https://www.w3schools.com/react/react_lifecycle.asp)

React의 각 컴포넌트는 3가지 주요 단계를 통해 주기를 관리하고 관찰할 수 있다.

3가지 주기란, Mounting, Updating, Unmounting을 의미한다.

# Mounting

Mounting이란 DOM에 요소를 붙이는 것을 말한다.

React는 컴포넌트를 마운팅할 때 순서대로 4가지 내장 메소드를 호출하게 된다.

1. `constructor()`
2. `getDerivedStateFromProps()`
3. `render()`
4. `componentDidMount()`

render() 메소드는 항상 호출되야하고, 다른 메소드들은 선택적으로 호출 될 수 있다.

---

## 1. constructor

`constructor()` 메소드는 컴포넌트가 초기화될 때 다른 어떤 메소드보다 먼저 호출되며, `state` 와 다른 초기 값들을 세팅한다.

`constructor()` 메소드는 `props` 라고도 불리며, `super(props)` 를 가장 먼저 호출해야 한다. `super(props)` 는 부모의 constructor 메소드를 초기화하고, 부모로 부터 상속받은 메소드들을 컴포넌트로 하여금 사용할 수 있도록 해준다.

### 예시 코드

```jsx
class Header extends React.Component {
  constructor(props) {
    super(props)
    this.state = { favoritecolor: 'red' }
  }
  render() {
    return <h1>My Favorite Color is {this.state.favoritecolor}</h1>
  }
}

ReactDOM.render(<Header />, document.getElementById('root'))
```

역자 주)

- Header 컴포넌트는 `constructor()` 를 통해 React.Component를 상속 받아 상속 받은 props와 state값을 저장한다.
- state 객체의 초깃값은 favoritecolor 속성을 `"red"`로 지정한다.

## 2. getDerivedStateFromProps

`getDerivedStateFromProps()` 메소드는 DOM에서 요소들이 랜더링 되기 직전에 호출된다.

최초의 `props` 에 기반한 `state` 객체를 저장한다.

`state` 를 인자로 받고, `state` 가 변하면 객체를 반환한다.

### 예시 코드

```jsx
class Header extends React.Component {
  constructor(props) {
    super(props)
    this.state = { favoritecolor: 'red' }
  }
  static getDerivedStateFromProps(props, state) {
    return { favoritecolor: props.favcol }
  }
  render() {
    return <h1>My Favorite Color is {this.state.favoritecolor}</h1>
  }
}

ReactDOM.render(<Header favcol="yellow" />, document.getElementById('root'))
```

역자 주)

- `getDerivedStateFromProps()` 는 props와 state를 인자로 받아 props의 favcol 값을 state의 favoritecolor 속성 값으로 세팅한다.

## 3. render

`render()` 메소드는 필수값이며, DOM에 HTML을 표현해준다.

### 예시 코드

```jsx
class Header extends React.Component {
  render() {
    return <h1>This is the content of the Header component</h1>
  }
}

ReactDOM.render(<Header />, document.getElementById('root'))
```

역자 주)

- `render()` 함수로 반환되는 html을 root DOM에 표현한다.

### 4. componentDidMount

`componentDidMount()` 메소드는 컴포넌트가 랜더링된 직 후에 호출된다.

이미 DOM에 위치한 컴포넌트를 필요로 하는 구문을 사용하는 곳이다.

### 예시 코드

```jsx
class Header extends React.Component {
  constructor(props) {
    super(props)
    this.state = { favoritecolor: 'red' }
  }
  componentDidMount() {
    setTimeout(() => {
      this.setState({ favoritecolor: 'yellow' })
    }, 1000)
  }
  render() {
    return <h1>My Favorite Color is {this.state.favoritecolor}</h1>
  }
}

ReactDOM.render(<Header />, document.getElementById('root'))
```

역자 주)

- `componentDidMount()` 를 통해Header 컴포넌트가 `랜더링 된 직후` 1초 후에 state 값을 변경한다. (favoritecolor ⇒ 'yellow')

# Updating

그 다음 단계인 Updating은 컴포넌트가 업데이트 할 때를 의미한다.

컴포넌트는 상태나 props가 변경될 때 마다 업데이트 된다

5가지 내장 메소드가 있고, 컴포넌트가 업데이트 되면 순서대로 실행된다.

1. `getDerivedStateFromProps()`
2. `shouldComponentUpdate()`
3. `render()`
4. `getSnapshotBeforeUpdate()`
5. `componentDidUpdate()`

---

### 1. getDerivedStateFromProps

update 단계에서 `getDerivedStateFromProps` 메소드가 호출된다. 이 메소드는 컴포넌트가 업데이트 될 때 제일 먼저 호출되는 메소드이다.

초기 props에 기반한 `state` 가 저장되는 곳이다.

### 예시 코드

```jsx
//If the component gets updated, the getDerivedStateFromProps() method is called:

class Header extends React.Component {
  constructor(props) {
    super(props)
    this.state = { favoritecolor: 'red' }
  }
  static getDerivedStateFromProps(props, state) {
    return { favoritecolor: props.favcol }
  }
  changeColor = () => {
    this.setState({ favoritecolor: 'blue' })
  }
  render() {
    return (
      <div>
        <h1>My Favorite Color is {this.state.favoritecolor}</h1>
        <button type="button" onClick={this.changeColor}>
          Change color
        </button>
      </div>
    )
  }
}

ReactDOM.render(<Header favcol="yellow" />, document.getElementById('root'))
```

역자 주)

- 위 컴포넌트에서 버튼을 클릭하면 onClick 이벤트로 favoritecolor를 'blue'로 변경해주지만, update 직후 실행되는 `getDerivedStateFromProps` 메소드로 인해 props favcol로 전달받은 'yellow'가 여전히 state 값으로 남아있게 된다.

## 2. shouldComponentUpdate

`shouldComponentUpdate()` 메소드는 React가 랜더링을 계속해야하는지 마는지에 대한 Boolean 값을 반환한다.

기본 값은 `true`이다.

### 예시 코드

```jsx
class Header extends React.Component {
  constructor(props) {
    super(props)
    this.state = { favoritecolor: 'red' }
  }
  shouldComponentUpdate() {
    return false
  }
  changeColor = () => {
    this.setState({ favoritecolor: 'blue' })
  }
  render() {
    return (
      <div>
        <h1>My Favorite Color is {this.state.favoritecolor}</h1>
        <button type="button" onClick={this.changeColor}>
          Change color
        </button>
      </div>
    )
  }
}

ReactDOM.render(<Header />, document.getElementById('root'))
```

역자 주)

위 컴포넌트에서 `shouldComponentUpdate()` 값이 false로 되어 있으므로 컴포넌트는 업데이트 되지 않는다.

## 3. render

컴포넌트가 업데이트 되면 역시 `render()` 도 호출된다. 새로운 변경점들을 갖고 리랜더링 된다.

## 4. getSnapshotBeforeUpdate

`getSnapshotBeforeUpdate()` 메소드는 업데이트 되기 전의 props와 state에 접근할 수 있다. 즉, update 이후에도 update 이전의 값들을 확인할 수 있다는 것을 의미한다.

만약 `getSnapshotBeforeUpdate()` 메소드가 존재하면, `componentDidUpdate()` 메소드도 포함해야 한다. 그렇지 않으면 에러가 발생한다.

### 예시 코드

```jsx
class Header extends React.Component {
  constructor(props) {
    super(props)
    this.state = { favoritecolor: 'red' }
  }
  componentDidMount() {
    setTimeout(() => {
      this.setState({ favoritecolor: 'yellow' })
    }, 1000)
  }
  getSnapshotBeforeUpdate(prevProps, prevState) {
    document.getElementById('div1').innerHTML =
      'Before the update, the favorite was ' + prevState.favoritecolor
  }
  componentDidUpdate() {
    document.getElementById('div2').innerHTML =
      'The updated favorite is ' + this.state.favoritecolor
  }
  render() {
    return (
      <div>
        <h1>My Favorite Color is {this.state.favoritecolor}</h1>
        <div id="div1"></div>
        <div id="div2"></div>
      </div>
    )
  }
}

ReactDOM.render(<Header />, document.getElementById('root'))
```

역자 주)

- 초깃값이 'red로 랜더링 되면, `componentDidMount()` 메소드에 의해 1초 후 값이 'yellow'로 업데이트 되고, 그 다음에 `getSnapshotBeforeUpdate()`메소드가 실행되며 div1에 해당 텍스트가 출력된다.
- 업데이트 이전 내역을 표시해 주었으므로 업데이트 이후도 실행되야 하기 때문에 `componentDidUpdate()` 가 포함되야 한다.

## componentDidUpdate

`componentDidUpdate()` 메소드는 컴포넌트가 DOM에서 update된 후에 호출된다.

### 예시 코드

```jsx
class Header extends React.Component {
  constructor(props) {
    super(props)
    this.state = { favoritecolor: 'red' }
  }
  componentDidMount() {
    setTimeout(() => {
      this.setState({ favoritecolor: 'yellow' })
    }, 1000)
  }
  componentDidUpdate() {
    document.getElementById('mydiv').innerHTML =
      'The updated favorite is ' + this.state.favoritecolor
  }
  render() {
    return (
      <div>
        <h1>My Favorite Color is {this.state.favoritecolor}</h1>
        <div id="mydiv"></div>
      </div>
    )
  }
}

ReactDOM.render(<Header />, document.getElementById('root'))
```

역자 주)

- 앞서 설명한 내용에 따라 초깃값 'red' 에서 'yellow'로 값이 업데이트 되고 그 내용이 mydiv DOM에 출력된다.

# Unmounting

그 다음은 컴포넌트가 DOM을 제거하거나 Unmounting 할 때를 의미한다.

React는 unmount 될 때 에서 단 하나의 메장 내소드를 갖고 있다.

- `componentWillUnmount()`

---

## componentWillUnmount

컴포넌트가 DOM에서 제거될 때 호출되는 메소드이다.
