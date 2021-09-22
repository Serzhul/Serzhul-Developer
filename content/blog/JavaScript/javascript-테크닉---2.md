---
title: Javascript 테크닉 - 2
date: 2021-09-21 15:09:62
category: JavaScript
thumbnail: { thumbnailSrc }
draft: false
---

# 1. Bitwise operator (~)

배열의 index에서 비트연산자 ~ 는 -1을 제외한 truthy한 값을 리턴한다

```jsx
// 비트 연산자 미적용
if(arr.indexOf(item) > -1) {

// 비트 연산자 적용
if(~arr.indexOf(item)) {
}
```

```jsx
function foundIndex(index) {
  return Boolean(~index)
}

const numbers = [1, 3, 5, 7, 9]

console.log(foundIndex(numbers.indexOf(5))) // true
console.log(foundIndex(numbers.indexOf(8))) // false
```

- 위와 같은 방식으로 ~ 연산자를 응용해서 사용할 수도 있다.

# 2. Object.entries() / Object.values()

Object.entries()는 객체에 담긴 키/값 들을 배열에 짝으로 변환한다.

```jsx
const versus = { player: 'Serzhul', hero: 'Jaina', enemy: 'Garrosh' }
const arr = Object.entries(versus)
console.log(arr)
/*
[ [ 'player', 'Serzhul' ],
  [ 'hero', 'Jaina' ],
  [ 'enemy', 'Garrosh' ]
]
*/
```

> Object.values()는 Object.entries()와 같은 기능이지만 키 없이 값만 변환한다.

```jsx
const versus = { player: 'Serzhul', hero: 'Jaina', enemy: 'Garrosh' }
const arr = Object.values(versus)
console.log(arr)
// [ 'Serzhul', 'Jaina', 'Garrosh' ]
```

# 3. Entries to Objects

Entries 처럼 배열에 키/값 형태로 저장된 배열들을 다시 객체로 만드는 테크닉

```jsx
const list = arr.map(person =>
  Object.assign(...person.map(([key, value]) => ({ [key]: value })))
)
```

# 4. Short-circuit evaluation (|| operator)

### If 조건문 없이 값 설정/확인하기

- 연산자 || 기준으로 왼쪽값이 false(undefined, null, '', 0, false 등)일 경우 값이 true인 오른쪽값을 리턴한다. (왼쪽값 || 오른쪽값)
- 즉, 오른쪽에 있는 값을 기본값(default)으로 설정한다고 생각하면 된다.

```jsx
var person = {
  name: 'Serzhul',
  age: 31,
}
console.log(person.job || 'unemployed')
// 'unemployed'
// person.job = false, 'unemployed' = true
// job 속성이 정의되어 있지 않아 false이므로, 'unemployed'가 출력된다.

var person = {
  name: 'Serzhul',
  age: 31,
  job: 'FE Developer',
}
console.log(person.job || 'unemployed')
// teacher
// person.job = true, 'unemployed' = true
// 'unemployed'가 true인지 확인하기 전에 person.job이 true이기 때문에 바로 그 값을 리턴한다.

var a
var b = null
var c = undefined
var d = 4
var e = 'five'

var f = a || b || c || d || e

console.log(f)
// 4    e까지 도달하기 전에 d가 true이기 때문에 4 출력
```

### Null이나 Undefined로 나타날 때 기본값으로 설정이 가능함

```jsx
// || 연산자 적용 안했을 경우
let english
if (test.score.ENGLISH) {
  english = test.score.ENGLISH
} else {
  english = 0
}

// || 연산자 적용했을 경우

const english = test.score.ENGLISH || 0
```

### Null, Undefined, 또는 empty('') 를 확인하면서, 변수에 저장하고 싶은 경우

```jsx
// || 연산자 미적용
if (variable1 !== null || variable1 !== undefined || variable1 !== '') {
  let variable2 = variable1
}

// || 연산자 적용
const variable2 = variable1 || 'new'
```

# 5. Create tally with .reduce()

### 배열 안 각 요소 개수를 객체로 변환하기

```jsx
const fruitBasket = [
  'banana',
  'cherry',
  'orange',
  'apple',
  'cherry',
  'orange',
  'apple',
  'banana',
  'cherry',
  'orange',
  'fig',
]

const count = fruitBasket.reduce((tally, fruit) => {
  tally[fruit] = (tally[fruit] || 0) + 1 // tally[fruit]가 없으면 = 0, 있으면 +1을 해준다     기본값이 0
  //tally[fruit] = (tally[fruit] + 1) || 1  : tally[fruit]가 있으면 + 1을 해준다 없으면 tally[fruit] = 1 (기본값)
  return tally
}, {})

console.log(count)
// { banana: 2, cherry: 3, orange: 3, apple: 2, fig: 1 }
```
