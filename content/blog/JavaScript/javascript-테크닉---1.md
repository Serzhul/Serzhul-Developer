---
title: Javascript 테크닉 - 1
date: 2021-09-20 14:09:05
category: Javascript
thumbnail: { thumbnailSrc }
draft: false
---

공부하면서 새롭게 알게 됐거나, 자주 사용된다고 생각되는 테크닉들을 모아서 정리하기 위한 포스트입니다.

# 1. DOM control 하기

```jsx
const example = document.querySelector('.example') // className이 example인 첫번째 dom 선택
const example2 = document.querySelectorAll('.example') // className이 example인 모든 dom을 array형태로 선택
const example3 = document.getElementById('example') // idName이 example인 dom 선택
```

# 2. localStorage 활용하기

```jsx
const EXAMPLE = 'EXAMPLE'

// API로 받은 오브젝트를 스트링화하여 localStorage에 저장
function saveExample(arr) {
  localStorage.setItem(EXAMPLE, JSON.stringify(arr))
}

// localStorage에 저장되어 있는 String을 다시 parse하여 object로 가져옴
function loadExample() {
  const loadedExample = JSON.parse(localStorage.getItem(EXAMPLE))
}
```

# 3. API fetch 하기

```jsx
function getData(address) {
	const fetchedData = fetch(address)
	.then((res) => res.JSON())
	.then((data) => {
		return data
		// 가져온 data가 Object형태로 담겨짐
}
}
```

# 4. Arrow function

```jsx
// 일반 함수 사용 방식
function sayHello(name) {
  console.log('Hello', name)
}
```

```jsx
// 화살표 함수 적용 방식
sayHello = name => console.log('Hello', name)
```

# 5. Default Parameter

Parameter의 기본값 설정이 가능하다

```jsx
// Default Parameter를 적용 안했을 때
function volume(l, w, h) {
  if (w === undefined) w = 3
  if (h === undefined) h = 4
  return l * w * h
}
```

```jsx
// Default Parameter를 적용 했을 때
volume = (l, w = 3, h = 4) => l * w * h
volume(2) // 24 = 2 * 3 * 4  (l=2)
```

# 6. 오늘 날짜 구하기

```jsx
// toLocaleDateString() 메소드 사용

let today = new Date()

console.log(today.toLocaleDateString('ko-KR')) // 20xx.xx.xx. 형태로 출력

const options = {
  weekday: 'long',
  year: 'numeric',
  month: 'long',
  day: 'numeric',
}

console.log(today.toLocaleDateString('ko-KR', options)) // 20xx년 xx월 xx일 x요일
```

```jsx
// 모든 옵션 형태
{
  weekday: 'narrow' | 'short' | 'long',
  era: 'narrow' | 'short' | 'long',
  year: 'numeric' | '2-digit',
  month: 'numeric' | '2-digit' | 'narrow' | 'short' | 'long',
  day: 'numeric' | '2-digit',
  hour: 'numeric' | '2-digit',
  minute: 'numeric' | '2-digit',
  second: 'numeric' | '2-digit',
  timeZoneName: 'short' | 'long',

  // Time zone to express it in
  timeZone: 'Asia/Shanghai',
  // Force 12-hour or 24-hour
  hour12: true | false,

  // Rarely-used options
  hourCycle: 'h11' | 'h12' | 'h23' | 'h24',
  formatMatcher: 'basic' | 'best fit'
}
```
