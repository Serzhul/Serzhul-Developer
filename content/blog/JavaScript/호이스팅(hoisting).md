---
title: 호이스팅(Hoisting)
date: 2021-04-09 00:04:89
category: JavaScript
thumbnail: { thumbnailSrc }
draft: false
---

- 호이스팅이란, 실제로 선언되기 전에 몇몇 타입의 변수들에 접근 가능하고 사용가능하게 만드는 것을 의미한다.(변수들을 그들의 스코프에 가장 상위에 위치시키는 것)

- 얼핏 보면 변수들이 마법처럼 스코프의 최상위로 이동하는 것처럼 보이지만 내부 동작 방식은 다음과 같다.

— 코드가 실행되기 전, 각각의 변수들에 대한 변수 선언문을 전부 스캔한 후, 변수 환경 객체(variable environment object)에 새로운 프로퍼티가 생성된다.

### 각 타입 변수들의 호이스팅 방식

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/085899c5-6fee-4a11-ab69-047c61c5c09f/hoisting.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/085899c5-6fee-4a11-ab69-047c61c5c09f/hoisting.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210409%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210409T145836Z&X-Amz-Expires=86400&X-Amz-Signature=2c9b2102399a47674e2d62e88cda521494d125a58c2b2cba21d0eb09ecc6eac8&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22hoisting.png%22)

### TDZ(Temporal Dead Zone)와 let, const

- 자바스크립트는 Top-down 방식으로 코드를 동작하게 되는데, 만약 먼저 선언되지 않은 변수가 호출될 경우 해당 변수가 포함된 코드를 TDZ(Temporal Dead Zone)영역으로 인식하게 된다.
- TDZ는 일반적으로 정의는 되었지만, 사용할 수 없는 변수의 영역을 포함하며, 실행할 경우 마치 그 변수가 실제론 없는 것처럼 동작한다.
- TDZ 영역의 변수에 접근하여 호출하게 되면 참조에러 : `Cannot access ~~ before initialization` 오류가 발생한다.
- 반면, 아예 선언도 되지 않은 변수를 호출할 경우에는 참조에러: `~~ is not defined`라는 에러가 발생한다.

### TDZ가 필요한 이유

- 오류를 피하거나 파악하기 쉽게 만들어준다 : 실제 선언 전에 변수에 접근하는 방식은 매우 좋지 않은 습관이며 피해야 하기 때문이다.
- const 변수들이 실제 필요한 방식대로 작동하게 해준다.(const는 상수 변수로 한번 할당되면 새로운 값이 할당되는 것이 불가능한데, TDZ는 const가 제대로 동작할 수 있도록 도와주는 역할을 한다.)
