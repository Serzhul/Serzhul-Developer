---
title: intersectionObserver
date: 2021-04-04 19:04:76
category: javascript
thumbnail: { thumbnailSrc }
draft: false
---

`IntersectionObserver`(이하 io)는 대상 요소와 그 상위 요소 혹은 최상위 도큐먼트인 viewport와의 교차 영역에 대한 변화를 `비동기적`으로 감지할 수 있도록 도와준다. 쉽게 말하면, 감시영역에 감시대상인 요소가 포함이 되는지 안되는 지를 파악한다. 더 쉽게는 사용자 화면에 지금 보이는 요소인지 아닌지를 구별하는 기능을 제공한다고 할 수 있다.

```jsx
// Intersection Observer 작성 예시.

const io = new IntersectionObserver(callback, options) // 관찰자 초기화
io.observe(element) // 관찰할 대상(요소) 등록
```

위 예시 코드에서 보듯이 IO는 callback과 option이라는 두개의 인수를 받는다.

`callback`은 대상 요소의 화면에 보이는 부분 백분율이 역치(어떤 반응을 일으키는데 필요한 최소한의 자극, 값 영어로는 `threshhold`라고 한다)보다 클 때 호출할 함수를 의미하며

`option`은 옵저버를 조정할 수 있는 옵션 객체. 아무런 설정도 하지 않으면 옵저버는 문서의 뷰포트를 루트로 사용하고, 여백은 없고, 역치는 0이 된다.

위 설명은 MDN에서 번역한 용어들을 사용하는데 아무래도 사전적 용어로 번역하다보니 쉽게 와닿지 않는 부분이 있다. 즉 쉽게 풀어쓰면 callback은 사용자가 현재 보고 있는 화면에서 변화가 일어났을 때 호출하는 함수를 의미하는데, 이 변화라는 것이 역치(threshhold)값을 얼마나 설정하느냐에 따라 기준이 달라진다.

보통 역치는 백분율 단위로 표시되며 0 ~ 1 까지로 0.1 단위로 표시할 수 있는데 0.5로 역치를 설정하면 50%를 의미한다고 할 수 있다. 즉, 역치가 0.5이고 사용자가 보이는 화면이 이 50%값을 넘는 변화를 일으키면 callback에 해당하는 함수를 호출하는 개념이라고 할 수 있다.

# Callback(entries, observer)

Callback은 entries와 observer를 인수로 받는데, entries는 IntersectionObserverEntry의 인스턴스 배열을 의미하며, observer는 콜백이 실행되는 해당 인스턴스를 참조한다.

## Entries 속성들

---

- `boundingClientRect`: 관찰 대상의 사각형 정보([DOMRectReadOnly](https://developer.mozilla.org/en-US/docs/Web/API/DOMRectReadOnly))
- `intersectionRect`: 관찰 대상의 교차한 영역 정보([DOMRectReadOnly](https://developer.mozilla.org/en-US/docs/Web/API/DOMRectReadOnly))
- `intersectionRatio`: 관찰 대상의 교차한 영역 백분율(`intersectionRect` 영역에서 `boundingClientRect` 영역까지 비율, Number)
- `isIntersecting`: 관찰 대상의 교차 상태(Boolean)
- `rootBounds`: 지정한 루트 요소의 사각형 정보([DOMRectReadOnly](https://developer.mozilla.org/en-US/docs/Web/API/DOMRectReadOnly))
- `target`: 관찰 대상 요소([Element](https://developer.mozilla.org/en-US/docs/Web/API/Element))
- `time`: 변경이 발생한 시간 정보([DOMHighResTimeStamp](https://developer.mozilla.org/en-US/docs/Web/API/DOMHighResTimeStamp))

# Options

---

`root`

대상 요소의 조상인 [Element](https://developer.mozilla.org/ko/docs/Web/API/Element) 객체는 경계 사각형이 뷰포트로 간주된다. 만약 Root의 가시영역에 보이지 않으면 observer은 그것이 같은 객체내에 속하는 부분이라고 하더라도 가시적인 것으로 판단하지 않는다.

`rootMargin`

교차점을 계산할 때, 계산 목적으로 루트를 줄이거나 늘리는 경우, 루트의 [bounding_box](https://developer.mozilla.org/en-US/docs/Glossary/bounding_box)에 추가 할 오프셋 세트를 지정하는 문자열이다. 구문은 CSS [margin](https://developer.mozilla.org/ko/docs/Web/CSS/margin) 속성의 구문과 거의 동일하며, 기본 설정은 "0px 0px 0px 0px"이다.

|참조 : [Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API#the_root_element_and_root_margin)

`threshold`

관측 대상에 대한 전체 상자 영역(루트)에 대한 교차 영역의 비율을 지정하며, 0.0과 1.0 사이의 숫자 하나 혹은 숫자 배열입니다. 0.0 값은 대상의 단일 픽셀이라도 보여지면, 대상이 보이는 것으로 계산되는 것을 의미한다. 1.0은 전체 대상 요소가 표시됨을 의미한다.

|참조 : [Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API#the_root_element_and_root_margin)

# 관련 함수들

### observe()

대상 요소의 관찰을 시작한다.

### unobserve()

대상 요소의 관찰을 중지한다. 관찰을 중지할 하나의 대상 요소를 인수로 지정해야 합니다.

### disconnect()

IntersectionObserver 인스턴스가 관찰하는 모든 요소의 관찰을 중지한다.

### takeRecords()

IntersectionObserverEntry 객체의 배열을 반환합니다.
