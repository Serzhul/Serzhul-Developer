---
title: JavaScript 불변성(immutability)
date: 2021-09-23 08:09:01
category: JavaScript
thumbnail: { thumbnailSrc }
draft: false
---

mutate는 변화, mutable는 변화 가능한, mutability는 변화 가능성을 의미하는 단어이다.

### immutability

- 불변함을 의미하는 단어이다.
- 프로그래밍에서 immutability는 데이터의 원본이 훼손되는 것을 막는 것을 의미한다.

## 이름을 불변하게 하는 법

- JavaScript에서 일반적으로 변수를 선언하는 방법은 'var' 키워드를 사용하는 방식이었다.
- 그러나 이 같은 방식은 값이 변할 수 있기 때문에 원본 훼손에 대한 위험에 노출되어 있었다.
- 따라서 ES6 이후 문법에서는 const라는 키워드를 도입해 상수(항상 같은 변수) 개념을 도입해 immutability를 지킬 수 있도록 해주었다.

```jsx
var v = 1
// some code
v = 2
console.log('v: ', v) // v: 2 =>

/* 처음 v를 만든 개발자는 1값을 기대하고 만들었을 것이다. 
그러나 다른 개발자가 위 코드를 모르고 새로운 값을 할당하면서 immutability가 훼손됐다. */
```

```jsx
const c = 1;
//some code
c = 2; // TypeError: Assignment to constant Variable
/* 실행되기 전에 위와 같은 오류가 발생한다. immutability도 지키면서 오류도 쉽게 잡아낼 수 있게 되었다.
```

## 값을 불변하게 하는 법

- JavaScript에는 원시형 타입과, 객체형 타입을 이해할 필요가 있다.
- 원시형 타입은 (Number, String, Boolean, Null, Undefined, Symbol)이 있다.
- 객체형 타입은 (Object, Array, Function 등)이 있다.

### 원시형 타입

- 원시형 타입은 선언되는 순간 값이 메모리에 저장되고, **`변수가 그 값을 가리키게`** 된다.
- 따라서 같은 값을 가리키는 두 변수를 동등 비교연산자(===)로 비교했을 때 true값이 반환된다.
  - 같은 값을 가리킨다는 것을 의미한다.

```jsx
var v1 = 1
var v2 = 1
console.log(v1 === v2) // true
```

### 객체형 타입

- 객체형 타입은 선언되는 순간 값 자체가 아닌 `**새로운 메모리 주소를 생성**`해 저장하기 때문에 객체 별로 새로운 주소가 생성된다.
- 따라서 동등 비교를 했을 경우 그 내용이 같더라도 false 값이 반환된다.

```jsx
var obj1 = {
  name: 'Serzhul',
}

var obj2 = {
  name: 'Serzhul',
}
console.log(obj1 === obj2) // false
```

### 결론

- 원시형 타입은 값이 그 값 자체이므로, 값을 바꿀 수 없다. ⇒ 불변하다.
- 객체는 같은 객체라도 속성 값을 바꿀 수 있기 때문에 ⇒ 불변하지 않다.(가변성을 가진다)

## 객체를 불변하게 만드는 방법

### Object.assign(빈 객체, 복사하려는 객체)

```jsx
var obj1 = { name: 'Serzhul' }

var obj2 = Object.assign({}, obj1)

obj2.name = 'Daewon'

// obj1 => {name: 'Serzhul'}
// obj2 => {name: 'Daewon'}

// obj1의 불변성이 유지된다.
```

### Object.freeze(불변하게 만드려는 객체)

```jsx
var obj1 = { name: 'Serzhul' }
Object.freeze(obj1)
obj1.name = 'Daewon'
console.log(obj1)
// {name: 'Serzhul'} => 속성 값이 변하지 않음
```

### Nested object (중첩된 객체)

- Object.assign은 객체의 불변성을 지킬 수 있게 해주지만 Nested object의 불변성은 지켜지지 못함.
- 만약 Nested object의 불변성도 유지하고 싶다면, 속성 내의 값도 복사를 해줘야함

```jsx
var obj1 = { name: 'Serzhul', score: [1, 2] }
var obj2 = Object.assign({}, obj1)
obj2.score = obj2.score.concat() // score Array의 불변성이 유지된다.

obj2.score.push(3)

// obj1 = {name:'Serzhul', score:[1,2]}
// obj2 = {name:'Serzhul', score:[1,2,3]}
```

- freeze도 마찬가지로 nested 객체에 적용해야한다.

# 불변 함수

- 객체형 타입의 불변성을 지키면서 새로운 객체를 만드는 함수의 예시이다.
- Object assign을 통해 객체를 복사 후, 복사한 객체의 속성을 변경한 후 반환하는 방식으로 동작한다.

```jsx
function changeProperty(person) {
  person = Object.assign({}, person)
  person.name = 'Daewon'
}

var obj1 = { name: 'Serzhul' }
var obj2 = changeProperty(obj1)
console.log(obj1, obj2)

// obj1 = {name:'Serzhul'}
// obj2 = {name:'Daewon'}
```

# const와 freeze의 차이점

```jsx
const obj1 = { name: 'Serzhul' }
Object.freeze(obj1)
const obj2 = { name: 'Serzhul' }
obj1 = obj2 // const로 인해 typeError 발생
obj1.name = 'Daewon' // freeze로 인해 오류 발생
```

- 위 예시코드에서 볼 수 있듯이 한마디로 정리하면 const는 선언된 변수명이 다른 값을 가리키지 못하도록 막는 역할을 한다고 할 수 있고, freeze는 객체내 속성 값을 바꾸지 못하게 하는 역할을 한다고 보면 된다.

# 요약

- 불변함이 중요한 이유는 아무리 강조해도 지나치지 않지만, 그렇다고 가변이 나쁜 것은 아니다.
- 가변의 단점을 보완하기 위해 불변이 존재하는 것이기 때문
- 추가로 immer 같은 라이브러리를 사용하면 불변성을 유지하면서 성능상의 이슈도 해결할 수 있는 좋은 방법이다.
