---
title: DOM은 어떻게 동작하는가?
date: 2021-05-09 09:05:58
category: JavaScript
thumbnail: { thumbnailSrc }
draft: false
---

# DOM(Document Object Model)

- DOM은 모든 JS와 Brower간에 호환되는 인터페이스이다. 좀 더 자세히 말하자면, 브라우저에서 랜더링되는 HTML Document들을 의미한다.
- DOM을 통해 우리는 자바스크립트로 HTML요소들을 만들거나, 수정하거나, 삭제할 수 있다.
- 또한 스타일을 설정하고 클래스나 속성을 설정할 수도 있으며 이벤트를 연결할 수도 있다.
- html document로부터 DOM tree가 생성되며 이 DOM tree로 상호작용할 수 있다.
- DOM은 DOM tree와 상호작용되는 수많은 메소드와 속성들을 갖고 있는 복잡한 API이다.

```jsx
// Example of dom methods and properties
.querySelector() / .addEventListener() / .createElement() /
.innerHTML / .textContent / .children / etc...
```

# DOM API의 내부구조

- DOM tree에서 각각의 객체는 노드라는 이름의 타입의 객체로 생성된다.
- 이러한 노드 객체들은 각각의 메소드나 속성(`.textContent, .childNodes, .parentNode, cloneNode() 등`)에 접근이 가능하다.
- 노드들에는 하위타입으로 `Element, Text, Comment, Document` 타입이 존재한다.

### Element

html태그로 대표되는 요소들을 의미하며 다음과 같은 메소드나 속성에 접근이 가능하다.

- `.innerHTML, .classList, .children, .parentElement, .append(), .remove() , .insertAdjacentHTML(), .querySelector(), .closest(), .matches(), .scrollIntoView(), .setAttribute()`
- Element의 하위타입으로는 `HTMLElement`가 있으며, HTMLElement는 HTMLButtonElement, 같은 Element들이 존재하며 각각의 특징과 속성들이 있다.

### Text

Element 안의 텍스트를 포함하는 일반적인 텍스트 값 모두가 해당된다.

### Comment

HTML Document에서 주석처리한 값들을 의미하며 `<!—Comment—>`의 형태로 사용된다.

### Document

`.querySelector(), .createElement(), .getElementById()` 같은 메소드들을 포함하며, document내의 다른 element들에 접근할 수 있는 타입이라고 할 수 있다.

— 이러한 Node의 메소드와 속성들은 상위부터 하위 타입들에게 상속된다. 즉 부모 타입으로부터 받은 모든 메소드나 속성값들은 하위 타입에서 사용할 수 있다.

- Node에는 앞서 언급한 4개의 하위타입 외에도 EventTarget(`.addEventListener(), .removeEventListener()` 포함 )이라는 상위 타입이 존재하며, EventTarget 하위에는 Node말고도 Window 타입이 존재한다.
- Window는 DOM과 관련이 없는 메소드들과 속성값들을 갖고 있다.
- 상속 규칙에 의해 Node 타입 및 하위 타입들은 EventTarget 타입의 메소드들에 접근이 가능하다.
