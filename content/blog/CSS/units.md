---
title: Units (단위)
date: 2021-03-23 22:03:08
category: CSS
thumbnail: { thumbnailSrc }
draft: false
---

- Px : 해상도에 따라 달라지는 기본 표현 단위 일반적으로 크기의 절대값을 표현하는데 사용된다.
- % : 부모 요소의 상대적 크기에 따른 비율을 나타내는 단위
- em : 부모 요소의 폰트 사이즈에 따른 크기 비율을 나타내는 단위

Ex) 부모 width 600, font-size : 10px이라면

300px 크기를 나타내기 위해 50%, 30em 으로 표현할 수 있다.

- Rem : 중간에 부모요소의 폰트 사이즈를 정의하기 애매할 때 최상위 조상요소(html-body 태그)의 폰트 사이즈를 기준으로 크기 비율을 나타내는 단위
- vw, vh : viewport를 기준으로 하는 크기 비율을 나타내는 단위로 vw는 너비(width) vh는 높이(height)를 나타낸다. 또한 기본적으로 백분율로 표현하여 50vw이면 viewport width의 50%를 의미한다.
- Vmax : 뷰포트에서 더 긴 단위 쪽을 단위로 계산
- Vmin : 더 짧은 단위 쪽을 단위로 계산

```css
// child의 너비와 높이를 각각 300px로 표현하는 예제

.container {
  width: 600px;
  height: 300px;
  background: tomato;
  margin: 15px;
  font-size: 10px;
}

.child {
  width: 50%;
  height: 30em;
  background: red;
}
```
