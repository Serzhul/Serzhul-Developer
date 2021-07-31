---
title: 동기(Synchronous)와 비동기(Asynchronous)
date: 2021-07-31 22:07:92
category: JavaScript
thumbnail: { thumbnailSrc }
draft: false
---

- 자바스크립트에서 대부분의 코드들은 동기적(Synchronous)이다. 동기적이란 뜻은 한줄 한줄 차례로 실행이 된다는 뜻이다.
- 즉 한 코드가 실행되고 나면 실행 쓰레드에 그 실행한 구문이 쌓이게 되고 각 라인의 코드들은 바로 전의 라인의 실행이 끝나기를 기다린다.
- 그러나 이 같은 방식은 한가지 문제점을 야기할 수 있다. 하나의 코드의 실행이 무척이나 오래걸릴 경우 그 다음에 대기하고 있는 코드들의 실행이 불가능하기 때문이다.

  ex) 가령 alert를 실행했을 경우, alert를 종료하거나 확인하지 않으면 아무것도 실행이 되지 않는다.

- 대부분의 경우에는 동기적 코드여도 괜찮지만, 위에서 언급한 문제점을 해결할 필요가 있는데 그 때 등장하는 것이 비동기적 코드이다.

### 비동기적 코드의 예시1

```jsx
const p = document.querySelector('.p')
setTimeout(function() {
  p.tesxtContent = 'My name is Serzhul!'
}, 5000)
p.style.color = 'red'
```

- 위와 같은 코드가 있다고 가정했을 때, setTimeout은 콜백함수로써, 5초 뒤에 p의 textContent를 위와같이 바꾸는 작업을 한다.
- 이 때 setTimeout이라는 비동기적 함수를 사용함으로써 자바스크립트의 실행 스레드는 그 다음 코드의 실행을 한다.(즉, p dom의 색깔이 빨간색으로 바뀐다)
- 비동기적 코드는 내부에서 작업이 끝난 다음에 작동한다.(예시의 경우 5초를 기다린다는 것) 그렇기 때문에 blocking이 되지않고, 스레드에서도 이전 동작이 끝날때까지 기다리지 않는다.
- 주의해야할 점은 콜백 함수 단독으로는 코드가 비동기적으로 작동하지 않는다. 따라서 비동기적으로 사용되는 특정 콜백함수들을 이해할 필요가 있다.

### 비동기적 코드의 예시2

```jsx
const img = document.querySelector('.dog')
img.src = 'dog.jpg' // 비동기적 코드
img.addEventListener('load', function() {
  img.classList.add('fadeIn')
})
p.style.width = '300px'
// 이벤트와 콜백의 비동기적 이미지 로딩
```

- 예시2에서는 2번째 라인에서 source 속성에 이미지를 설정하는데, 이 과정이 비동기적으로 작동한다. 속성에 이미지를 설정한다는 것은 백그라운드에서 이미지를 불러온다는 것으로 이해하면 된다. 그러므로 이미지를 불러오는 동안 그 뒤의 나머지 코드들이 실행된다.
- 이미지가 다 실행되고나면 load 이벤트가 실행이되며 이미지태그의 클래스에 fadeIn이 추가된다.
- 위에서 언급했듯이 콜백 함수가 단독으로 쓰이면 비동기적으로 실행되지 않는 것처럼, addEventListenr역시 마찬가지이다.

## AJAX(Asynchronous Javascript And XML)

- AJAX는 원격 웹서버와 비동기적으로 상호작용을 할 수 있도록 도와주는 방식이다. AJAX를 콜함으로써 웹서버에 데이터를 동적으로 요청할 수 있다.
- 대표적으로 페이지를 리로드하지 않고 데이터를 상호작용하는 방식이 있다.
- AJAX는 이름에서도 알 수 있듯이 XML을 사용해 데이터를 담았지만 최근에는 JSON 방식이 훨씬 많이 사용된다.

### API(Applicataion Programming Interface)

- 일반적으로 클라이언트가 Request(특정 데이터를 요청하는)를 웹서버에 보내면 웹서버에서는 데이터를 담아 Response(web API)한다.
- API란, 다른 소프트웨어의 부분으로 사용될 수 있는 소프트웨어로써, 어플리케이션이 상호작용할 수 있도록 만들어주는 소프트웨어이다.
- API의 종류는 무수히 많다. (`DOM API`, `위치 API` , `Own Class API`, `Online API`등)
- `Online API` 란, 서버에서 작동하며, 데이터 요청을 받아 다시 Response로 데이터를 보내주는 API를 말한다. 일반적으로 말하는 API는 이 Online API를 의미한다.
- API는 개발자가 직접 만들수도 있고, 서드파티의 API를 사용할 수도 있다.
