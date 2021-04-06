---
title: Offset, Client, Scroll 개념 정리
date: 2021-04-03 22:04:39
category: JavaScript
thumbnail: { thumbnailSrc }
draft: false
---

JS와 React를 공부하다보면 가장 기초적인 부분의 개념이 부족하다고 느낄 때가 있다. 최근 공부하고 있는 scroll 이벤트나 resize 이벤트를 공부하다보니 브라우저나 스크롤의 크기를 코드상으로 어떻게 계산하고, 각각의 속성들의 의미하는 개념들은 기본적으로 알고 있어야 겠다는 마음이 들었다. 물론 이 개념들을 완벽히 암기, 숙지하기란 너무나 비효율적이며 그때 그때 구글링을 통해 파악하긴 하겠지만 한번 제대로 정리 해놓으면 다시 개념을 봤을 때 좀 더 이해가 빠를 것이라 생각한다.

# 요소의 크기에 대한 속성들

처음 html css를 접하면 기본적으로 요소의 크기에 대해 width와 height, 더 나아가 border, padding, margin 등의 개념을 배우지만 이 개념은 요소의 크기를 나타내는 극히 일부분에 불과하다. 복잡하게 들어가면 브라우저와의 위치관계, scroll 간의 위치관계 등 좀 더 거시적으로 요소의 위치와 크기 관계를 파악할 수 있는데 이번에 다룰 offset, client, scroll과 관계된 속성값들이 그것이다.

# offsetParent, offsetLeft, offsetTop

우리가 작성하는 코드들은 전부 영어 베이스이기 때문에 영어를 잘하는 사람은 처음 들어보는 개념이라도 이름만으로도 그 개념의 특성을 어느정도 짐작할 수 있다.(물론 그렇지 않은 것도 많지만 꽤 많은 개념들이 이에 해당한다.) 나도 프로그래밍을 공부하면서 그런 재미를 느껴 새로운 개념을 배우게 되면 그 단어의 사전적 의미를 먼저 파악하고 그것이 프로그래밍 개념으로 어떻게 표현되는지를 알아내는 습관이 들었다.

그래서 그런지 offset이란 말을 들었을 때 상쇄하다라는 사전적의미만 알고 있던 나에게 offsetParent, offsetLeft, offsetTop 같은 개념들은 얼핏 이해가 되지 않았다. 그런데 offset영역이 화면에서 보여지는 요소의 전체 영역을 의미한다는 것을 알게 된 순간 다음과 같은 생각이 들었다.

**화면 전체와 요소와 상쇄되는 영역 ⇒ Offset 영역 ⇒ 요소의 전체 영역**

이런식으로 offset영역이란 요소의 전체영역을 의미한다고 인지하면 이런식으로 풀어서 이해할 수 있고, 쉽게 다른 속성들과의 관계도 떠올릴 수 있었다.

먼저, offset영역이 요소 전체의 영역이라면, offsetParent는 바로 요소의 부모의 전체영역이라고 할 수 있다.

MDN에 따르면, offsetParent는 요소를 렌더링할 때, 좌표 계산에 사용되는 가장 가까운 조상 요소(the closest positioned ancestor element)의 참조를 반환한다고 되어있다. 즉, 위치관계가 명확해야한다는 것인데, 이는 일반적으로 position 프로퍼티가 설정되어 있는 요소라는 것을 의미한다. position이 설정되어 있어야 가장 가까운지 여부를 판단할 수 있기 때문이다.

만약 position 프로퍼티가 설정되어 있는 조상 요소가 없는 경우 가장 가까운 조상 요소 중 `<tb><th><table>` 혹은 `<body>` 가 해당이 된다.

offsetParent의 개념이 이렇게 정리된다면 나머지 속성들은 이를 통해 충분히 유추할 수 있다.

`offsetLeft`, `offsetTop` 의 경우 각각 offsetParent를 기준으로 오른쪽으로, 아래쪽으로 얼마나 떨어져 있는지 나타낸다.

# clientLeft, clientTop, clientWidth, clientHeight

offset을 위처럼 이해하고 나니 다른 속성들도 같은 방식으로 이해하고 싶었다.

이번에는 client인데, client는 일반적으로 손님이라는 뜻이있지만 프로그램에서 사용하는 client의 개념은 Server와 반대되는 개념으로 사용되어 오히려 헷갈렸다.

그러나 client 영역이 border영역과 연관성이 있다는 것을 알게 된 후 다음과 같은 개념으로 유추할 수 있었다.

**요소의 border영역 ⇒ 요소의 main이 아닌 sub 영역 ⇒ 주인이 아닌 손님(Client)의 영역**

이러한 유추방식이 정확하진 않을 수 있지만 이런식으로 연산하자 client 속성들의 개념이 보이기 시작했다.

먼저 `clientLeft`와, `clientTop`은 일반적으로 왼쪽 border의 너비, 위쪽 border의 너비와 같다. Client영역이 border영역과 같은 개념으로 보면 쉽게 이해할 수 있다. 그러나 위 정의가 항상 옳지는 않은데 그 이유는 좌우Scroll이나 상하Scroll이 생겼을 때 그 너비를 포함하는 개념이기 때문이다.

가령 한 요소의 좌측 테두리 길이가 30px이었다고 하면 스크롤이 없을 때는 clientLeft 값도 30px로 동일하다. 그런데 아랍어나 히브리어처럼 오른쪽에서 왼쪽으로 글이 전개되는 언어로 브라우저가 세팅되면 스크롤이 왼쪽 영역에 생기게 되고, clientLeft 값에는 스크롤의 길이만큼 값이 더해지게 된다.

| 참조 : ([https://ko.javascript.info/size-and-scroll](https://ko.javascript.info/size-and-scroll))

이런 경우는 매우 드문 경우지만, 항상 border길이와 같지 않다는 선에서 이해하면 괜찮을 것이다.

반면 `clientHeight` 와 `clientWidth` 는 테두리를 제외한 즉, 요소의 테두리 안의 영역 사이즈 정보를 제공한다. 이때 요소 내부의 여백인 Padding 값은 포함되지만 스크롤바의 너비는 포함되지 않는다.

즉 이를 식으로 표현하면

`clientHeight = CSS상의 높이 + CSS상의 내부 여백 - 수평 스크롤바의 높이(존재하는 경우에만)`

`clientWeight = CSS상의 너비 + CSS상의 내부 여백 - 수직 스크롤바의 너비(존재하는 경우에만)`

으로 표현할 수 있다.

# scrollWidth, scrollHeight, scrollLeft, scrollTop

scroll 영역은 다행이 offset, client와 다르게 직관적으로 그 의미를 파악하기 쉬웠다.

`scrollWidth` 와 `scrollHeight` 는 clientWidth, clientHeight와 유사한데, scroll로 감춰진 영역을 포함한다는 점이 차이점이 있다. 따라서 스크롤바가 없어서 감춰진 영역이 없으면, clientWidth, clientHeight와 같은 값을 갖게 된다.

`scrollLeft` 와 `scrollTop` 은 스크롤바가 움직임으로 인해서 가려지는 너비와 높이를 의미한다.

만약 수직 스크롤을 100px 만큼 내리면 scrollTop 값은 100px이 되는 식이다.

### 요약

- `offsetParent` – 위치 계산에 사용되는 가장 가까운 조상 요소나 `td`, `th`, `table`, `body`
- `offsetLeft`와 `offsetTop` – `offsetParent` 기준으로 요소가 각각 오른쪽, 아래쪽으로 얼마나 떨어져 있는지를 나타내는 값
- `offsetWidth`와 `offsetHeight` – 테두리를 포함 요소 '전체’가 차지하는 너비와 높이
- `clientLeft`와 `clientTop` – 요소 제일 밖을 감싸는 영역과 요소 안(콘텐츠 + 패딩)을 감싸는 영역 사이의 거리를 나타냄. 대부분의 경우 왼쪽, 위쪽 테두리 두께와 일치하지만, 오른쪽에서 왼쪽으로 글을 쓰는 언어가 세팅된 OS에선 `clientLeft`에 스크롤바 두께가 포함됨
- `clientWidth`와 `clientHeight` – 콘텐츠와 패딩을 포함한 영역의 너비와 높이로, 스크롤바는 포함되지 않음
- `scrollWidth`와 `scrollHeight` – `clientWidth`, `clientHeight` 같이 콘텐츠와 패딩을 포함한 영역의 너비와 높이를 나타내는데, 스크롤바에 의해 숨겨진 콘텐츠 영역까지 포함됨
- `scrollLeft`와 `scrollTop` – 스크롤바가 오른쪽, 아래로 움직임에 따라 가려지게 되는 요소 콘텐츠의 너비와 높이
