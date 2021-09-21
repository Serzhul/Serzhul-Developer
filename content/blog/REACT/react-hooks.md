---
title: React Hooks
date: 2021-09-21 18:09:46
category: React
thumbnail: { thumbnailSrc }
draft: false
---

## React Hooks

React Hooks란, 함수형 컴포넌트에서만 사용되는 특수한 형태의 함수를 의미한다.

일반적으로 hooks는 다른 함수들과 착각하지 않기 위해 앞에 use를 붙여 만들어진다.

⇒ useState, useEffect(), useRef() ...

hooks를 함수형 컴포넌트에서 호출하면, hooks에 따라 기능이 가능해진다.

⇒ 가령, useState의 추가 기능은 함수형 컴포넌트에서 state를 관리 가능하도록 해준다.

# useState()

- useState는 React의 핵심 기능이라고도 할 수 있으며, state(상태)값을 제어하기 위해 사용되는 hook이다.

```jsx
import { useState } from 'react'

const someFunction = () => {
  useState() // call 해서 사용하며 파라미터로 초깃값을
}
```

- useState는 call해서 선언하는데 이때 파라미터로 초기값을 설정하게 된다. 초깃값으로는 어떠한 종류의 값이 와도 상관없다.
- useState를 선언하게 되면 이 안에 배열이 있고 그 배열 안에는 2가지 요소가 존재한다.. 첫번째는 내가 초깃값으로 선언한 값을 갖는 요소이며, 두번째는 이 값을 바꾸는 함수이다. 즉 이 함수를 사용해 초깃값으로 선언한 값을 update한다고 보면 된다.
- useState는 일반적으로 비구조화 할당을 통해 선언하게 되는데 사용법은 아래 예시와 같다.

```jsx
import { useState } from 'react'

const someFunction = () => {
  const [item, setItem] = useState({
    title: '',
    description: '',
  })
}

// item이라는 객체 state값과, 이를 업데이트 해주기 위한 함수 setItem을 비구조화 할당을 통해 선언
```

- useState를 사용할 때 주의할 점은 객체를 사용할 때, 특정 키 값만 update하면 안된다는 것이다. 바닐라 자바스크립트에서 객체를 재할당하면 병합이 되지만, useState에서 업데이트 함수로 특정 키값을 재할당하면, 새로운 객체로 대체되어 기존 키 값들은 사라지게 된다.

  ⇒ 이를 방지하기 위해서 closure을 통한 해결방법, 객체 state를 개별 state로 분리하는 방법 등이 있다.

### useState 이해하기

- 함수형 컴포넌트에서 useState를 사용하면 state가 생성되고 이를 바탕으로 React를 제어한다.
- useState는 2가지 요소를 반환하는데 첫번째는 State Pointer 즉, state 값을 가리키는 요소를 말하며, 둘째는 State update function이다.

### hooks의 중요한 규칙 2가지

1. Hooks는 함수형 컴포넌트에서만 사용하거나, 다른 custom hook 안에서만 사용해야 한다.
2. Hooks는 항상 컴포넌트의 root level에서만 사용해야 한다.
   - 가령 컴포넌트 내의 함수 안에서 사용하는 것은 불가능하다.

## useEffect()

- useEffect는 app이 마운트 될 때, 즉 모든 컴포넌트가 렌더링 되고 나서 실행되도록 만드는 작업을 제어한다.

```jsx
import useEffect from 'react'

const someFC = () => {
  useEffect(() => {})
}
```

- 또한 component가 업데이트 되면 다시 렌더링 하므로 매번 컴포넌트가 다시 렌더링 되고 실행된다.
- useEffect의 첫번째 인자는 실행되는 함수를 정의하고, 두번째 인자로는 dependency를 정의한다. Dependency의 역할은 해당 Dependency가 변화했을 때만 useEffect가 작동하도록 제어해주는 역할을 한다. 만약 비워두면 매 사이클 마다 useEffect가 실행되게 되므로, empty array를 설정하는것이 일반적이다.(처음 렌더링 될때 한번만 실행된다.)

## useCallback()

- useCallback을 이해하기 위해선 먼저 react의 사이클을 이해할 필요가 있다. react app의 컴포넌트들은 re-rendering될 때 컴포넌트 내의 함수들을 잃어버리고 새롭게 선언해서 실행하게 되는데, 이렇게 되면 쓸데 없는 리소스를 낭비하게 될 경우가 많다. 따라서 일부는 저장해두고 그냥 가져다 쓰기만 하도록 지정해줄 수 있도록 하는 것이 필요한데 그것이 바로 useCallback이다.
- useCallback은 두개의 인자를 받는다. 첫째는 실행함수이고, 두번째는 dependency이다. 이는 useEffect와 사용법이 유사하며, 용도만 다르다고 이해하면 된다.

```jsx
import { useCallback } from 'react'

const someFC = () => {
  const sumeFunc = useCallback(() => {}, [])
}
```

## useRef()

- useRef는 특정 DOM의 값을 가져오기 위해 참조할 DOM을 지정하는 용도로 사용되는 hook이다.

```jsx
import {useRef} from 'react'

const someFC = () => {
	const inputRef = useRef();

	const value = inputRef.current.value;

	console.log(value); // input태그에 입력된 값이 출력됨.

	return (
		<form>
			<input type="text" ref={inputRef}>
		</form>
	)
}

```

## useEffect Cleanup 함수

- Cleanup 함수란, useEffect 내의 함수가 여러번 실행될 때, 다음 useEffect가 실행되기 전에 실행되는 함수를 말한다.
- 만약 useEffect의 dependency가 []라면, cleanup 함수는 컴포넌트가 unmounted 될 때 실행된다.
- 사용법은 useEffect 함수가 실행된 마지막에 return 값으로 익명의 함수를 선언해 그 내용이 실행되도록 한다.
- Cleanup 함수를 사용하는 이유는 메모리 관리에 용이하다. 이 작업을 통해 보다 효율적으로 useEffect가 작동할 수 있도록 제어할 수 있기 때문이다.
