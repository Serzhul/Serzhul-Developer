---
title: JS로 DOM 그리는 3가지 방식
date: 2021-09-03 18:09:99
category: Javascript
thumbnail: { thumbnailSrc }
draft: false
---

DOM을 그리는 방식은 정말 다양하지만 대표적인 방식 3가지가 존재한다.

# 1. DOM API

- 가장 널리 사용되던 방식이며, DOM API를 사용해 내장된 함수로 DOM 객체를 생성하고 거기에 살을 붙여 구조를 만들어 나가는 방식이다.
- 대표적인 함수로는 다음과 같은 것들이 있다.
  - document.createElement()
  - document.getElementById()
  - document.querySelector()
  - document.appendChild()
  - document.insertAdjacentHTML()

```tsx
const createDOM = () => {
  const div = document.createElement('div')
  const ul = document.createElement('ul')

  for (let i = 1; i < 4; i++) {
    const li = document.createElement('li')

    li.textContent = i

    ul.appendChild(li)
  }

  div.appendChild(ul)

  document.body.appendChild(div)
}

createDOM()
```

# 2. Template Literal

- 1번 방법은 작은 규모의 어플리케이션에는 크게 문제되지 않지만 크기가 커질수록 HTML 구조를 한눈에 파악하기 어렵다는 단점이 있다. 이에 한 눈에 구조를 파악할 수 있도록 html을 문자열로 만들고, 배열에 담아 html의 innerHTML을 활용해 랜더링 하는 방법이 생겼다.

```tsx
const createDOM = () => {
  let template = []
  template.push('<div><ul>')
  for (let i = 1; i < 4; i++) {
    const list = `
    <li>${i}</li>
    `

    template.push(list)
  }
  template.push('</ul></div>')

  document.body.innerHTML = template.join('')
}

createDOM()
```

- 배열에 html 문자열을 순서대로 담고, 이를 합쳐서 innerHTML에 넣어준다. 예시 코드는 짧아서 잘 눈에 띄진 않지만 html이 보다 긴 코드라면 1번 방법보다 훨씬 쉽게 파악할 수 있다.

# 3. Template Replace

- 2번보다 더 개선되어 나온 것이 template Replace방식이다. 2번과 크게 다르지는 않지만

HTML 구조 전체를 한번에 볼 수 있고 필요한 부분만 대체되는 방식이기 때문에 더욱 구조 파악이 용이하다.

```tsx
const createDOM = () => {
  let list = []

  let template = `
  <div>
    <h1>test</h1>
    <ul>
      {{__li__}}
    <ul>
  </div>
  `

  for (let i = 1; i < 4; i++) {
    list.push(`<li>${i}</li>`)
  }

  template = template.replace('{{__li__}}', list.join(''))

  document.body.innerHTML = template
}

createDOM()
```
