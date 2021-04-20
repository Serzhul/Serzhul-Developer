---
title: Set과 Map
date: 2021-04-20 23:04:58
category: JavaScript
thumbnail: { thumbnailSrc }
draft: false
---

기존의 자바스크립트에서는 Array와 Object 구조만이 사용되었지만 ES6와 Set과 Map도 새롭게 도입되어 다른 언어에서 사용하듯이 JS에서도 사용할 수 있게 되었다.

# Set

Set의 가장 큰 특징은 Array와 달리 중복값을 갖지 않는다는 것이다. 중복값이 들어오면 자연스럽게 제거되며, 유니크한 값만 set내에 남게 된다.

또한 iterable한 값을 받아 표현가능하며, 내용물이 비어있는 set도 선언이 가능하다.

단, array와 달리 index가 존재하지 않으며 요소간 순서도 존재하지 않는다.

### Set의 사용예시

```jsx
const mySet = new Set()

console.log(mySet) // Set(0)

const newSet = new Set('hehehe')

console.log(newSet) // Set(2) {"h", "e"}

const newSet2 = new Set([
  'pizza',
  'pasta',
  'pizza',
  'chicken',
  'pizza',
  'hamburger',
])

console.log(newSet2) // Set(4) {"pizza", "pasta", "chicken", "hamburger"}
```

- Set의 길이는 `Set.size`로 출력할 수 있다.
- Set은 has메소드(`Set.has`)를 사용해 해당 요소를 갖고있는지 체크할 수 있다. (boolean)
- Set에 특정 요소를 추가할때는 add메소드를 사용한다. (`Set.add()`)
- Set에서 특정 요소를 제거할때는 delete메소드를 사용한다. (`Set.delete()`)
- Set의 모든 요소를 삭제할 때는 cleaer메소드를 사용한다. (`Set.clear()`)
- Array와 마찬가지로 Spread연산자, for... of 문이 사용가능하다.

# Map

Map의 가장 큰 특징은 object와 같이 key와 value를 갖는다는 점이지만, Map만의 차별점은 key값에 어떠한 타입(Array, Map, Set, DOM 등)이든 올 수 있다는 것이다. (Object는 키값으로 String만 가능하다)

### 사용예시

```jsx
const newMap = new Map()

newMap.set('name', 'Serzhul')
newMap.set(1990, 'birthYear')
newMap.set(true, 'programmer').set(false, 'Job')

console.log(newMap) // Map(4) {"name" => "Serzhul", 1990 => "birthYear", true => "programmer", false => "Job"}
```

- `Map.set()` 메소드는 Map에 새로운 요소를 추가하기도하지만 동시에 업데이트 된 Map을 리턴한다.
- `Map.set()` 메소드는 Chain해서 사용할 수 있으며, set을 call 할때마다 새로운 요소가 추가된 map으로 업데이트 된다.
- Map의 value를 가져올때는 `Map.get(key)` 를 사용한다.
- Map는 다음과 같은 메소드, 값들을 가진다.

— `Map.has(key)`, `Map.delete(key)`, `Map.clear()`, `Map.size`

- Object는 Iterable하지 않은데 반해 Map은 Iterable 하므로 가공하기 용이하다. 따라서, Object를 사용해 Map으로 전환해 가공하는 방법이 있는데 `Object.entries()` 을 사용하면 된다.

### 응용예시

```jsx
const newMap = new Map()

newMap.set([1, 2, 3], 'array')

console.log(newMap.get([1, 2, 3])) // Undefined
```

- 위 소스는 Map에서 배열이 key값으로 사용된 예시를 보여준다. 그러나 같은 키값을 호출했지만 console에는 undefined가 출력된다.
- 그 이유는 2번째 라인의 [1,2,3]이 가리키는 Array와 3번째라인에서 get으로 설정한 키값 [1,2,3] 배열은 똑같아 보이지만 다른 배열이기 때문이다.

이를 올바르게 나오도록 수정하려면 다음과 같이 수정하면 된다.

```jsx
const newMap = new Map()

const newArr = [1, 2, 3]

newMap.set(newArr, 'array')

console.log(newMap.get(newArr)) // 'array'
```

### 응용예시2

```jsx
// Map을 Object로 바꾸는 방법
const newMap = new Map()

const newObject = Object.entries(newMap)

// Map을 Array로 바꾸는 방법

const newArr = [...newMap]
const newKeys = [...newMap.keys()]
const newValues = [...newMap.values()]
const newEntries = [...newMap.entries()]
```
