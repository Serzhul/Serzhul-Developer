---
title: NextJS API Routes
date: 2021-10-14 16:10:09
category: React
thumbnail: { thumbnailSrc }
draft: false
---

# API Routes란?

- 몇몇 웹사이트는 단순히 페이지를 보여주는 것 외에 다른 기능들을 갖고 있다.
  - 유저들의 피드백을 받거나, 뉴스레터에 가입하거나 등등...
- 이런 경우 전달받은 데이터를 DB에 전송해 저장할 필요가 있는데 이 때 html page가 아닌 사용자로부터 입력받은 데이터만 필요로한다. 이 때 사용하는 것이 API이다.

### API(Application Programming Interface)란?

- API는 여러 종류가 있지만 그 중에서도 REST API가 가장 대표적이다.
- REST API는 Representational State Transfer의 약자로, REST 규칙의 특수한 구조를 가진 웹 API를 말한다. (여기서 자세한 내용은 다루지 않는다.

- API Routes는 HTML을 반환하지 않는 대신 (REST) API를 제공하는 URL을 말한다.(위에서 언급했듯이 우리는 데이터만 필요하고 html page는 필요하지 않으므로)
- Request는 단순히 URL을 입력해서 전송되는 것이 아니라, JavaScript code(Ajax)를 통해 들어가게 된다.

# API Routes 사용하기

- NextJS에서 API Routes를 사용하기 위해선 먼저 pages 폴더안에 api라는 이름의 폴더를 만든다. (다른 이름으로 작성하면 NextJS에서 인식하지 못한다.)
- 그 후에 js파일을 만드는데 이 때 만드는 파일은 자유롭게 만들어도 상관없다.
  - URL은 `domain / api / 이름` 형태로 작성된다.
- API Routes을 사용하기 위해서 함수를 하나 작성한다. 이 함수는 2개의 인수를 받는다(request, response)
  - NextJS는 request를 해당 URL(domain/api/이름)에 보내기 위해 이 함수를 실행한다.
  - 이 함수에서는 html 코드를 보낼 필요없이 서버사이드에서 진행할 작업에 대한 내용만 작성하면 된다. (클라이언트 사이드에서 노출되지 않는다.)

### 기본 작성 예시 1)

```jsx
function handler(req, res) {
  res.status(200).json({ message: 'Success' })
}
```

- 받아온 response 인자로 여러가지 메소드를 사용할 수 있는데, 대표적인 메소드가 status이다. response가 성공적으로 이뤄졌을 때 status가 200이 되므로 위와 같이 사용할 수 있다.
- 또한 여러개의 메소드를 체이닝해서 사용할 수도 있다. status다음에 오는 json은 api로 넘어오는 데이터를 json 포맷으로 변형해주는 역할을 한다.

## POST 방식으로 데이터 전송하기

- 데이터 전송이 잘 되는지 테스트 하기 위해 먼저 data를 저장할 폴더(top-level의 폴더)와 내부의 json파일을 생성하고 빈 배열을 작성한다.
- 그리고 index.js에 데이터를 보내기 위한 form과 submit함수를 작성한다.

### submit 함수 예시

```jsx
function submitFormHandler(event) {
  event.preventDefault()

  const enteredEmail = emailInput.current.value
  const enteredFeedback = feedBackInput.current.value

  const reqBody = {
    email: enteredEmail,
    text: enteredFeedback,
  }

  fetch('/api/feedback', {
    method: 'POST',
    body: JSON.stringify(reqBody),
    headers: {
      'Content-Type': 'application/json',
    },
  })
    .then(response => response.json())
    .then(data => console.log(data)) // {email : "test@test.com", text: 'Some feedback text'};
}
```

- 해당 함수는 form에서 email과 text값을 입력받아 보내는 로직이다.
- 여기서 주의할 점은 fetch로 데이터를 전송할때 URL이 우리가 위에서 설정한 API Routes의 주소라는 점이다. (pages - api 밑의 js파일이름)
- body는 객체로 만들어 JSON.stringify 메소드를 사용하여 전송한다.

## API Routes 함수 예시

```jsx
function handler(req, res) {
  // 어떤 종류의 request인지 check
  if (req.method === "POST") {
    const { email, text } = req.body;

    const newFeedback = {
      id: new Date().toISOString(),
      email,
      text,
    };

    // 데이터베이스에 저장하거나 파일에 저장한다.
    const filePath = path.join(process.cwd(), "data", "feedback.json");
    const fileData = fs.readFileSync(filePath);
    const data = JSON.parse(fileData);
    data.push(newFeedback);
    fs.writeFileSync(filePath, JSON.stringify(data));

    res.status(201).json({ message: "Success!", feedback: newFeedback });
  } else {
    res.status(200).json({ message: "Works" });
  }
```

- 기존에 작성했던 handler 함수에 다음과 같은 로직을 추가한다.
  - submit 함수를 통해 request가 오면 그 request의 method를 파악하는 분기를 추가한다.
  - body 데이터를 추출하여 변수로 만들고 파일에 데이터를 저장하는 로직을 추가한다.
    - 위 함수의 로직은 Node.js에서 파일을 읽어, 파일을 저장하는 방식이다.

### 로직 설명

- filePath는 process.cwd()를 통해 프로젝트 절대 경로를 가져오고, 그 밑의 data 폴더에 feedback.json 파일의 경로를 지정한다.
- fileData는 fs 시스템에 의해 filePath의 경로에 해당하는 파일을 읽어온다.
- data는 읽어온 파일을 JSON형태로 변형한다. (위에서 빈 배열을 작성했으므로, 배열이 들어간다.)
- data에 fetch로 받아온 데이터를 push한다.
- 배열을 다시 filePath에 해당하는 파일에 저장한다. (string으로 변형하여)
- 마지막으로 response에는 201 status(성공적으로 데이터를 저장했을 경우)와 message, 데이터를 담아 전송한다.

## GET 방식으로 데이터 받아오기

- 큰 틀은 POST와 동일하며, 기존에 작성한 submit 함수를 리팩토링하여 작성하면 다음과 같다.

### 함수 수정 예시

```jsx
function buildFeedBackPath() {
  return path.join(process.cwd(), 'data', 'feedback.json')
}

function extractFeedback(filePath) {
  const fileData = fs.readFileSync(filePath)
  const data = JSON.parse(fileData)
  return data
}

function handler(req, res) {
  // 어떤 종류의 request인지 check
  if (req.method === 'POST') {
    const { email, text } = req.body

    const newFeedback = {
      id: new Date().toISOString(),
      email,
      text,
    }

    // store that in a database or in a file;
    const filePath = buildFeedBackPath()
    const data = extractFeedback(filePath)
    data.push(newFeedback)
    fs.writeFileSync(filePath, JSON.stringify(data))

    res.status(201).json({ message: 'Success!', feedback: newFeedback })
  } else {
    const filePath = buildFeedBackPath()
    const data = extractFeedback(filePath)
    res.status(200).json({ feedback: data })
  }
}
```

- 로직이 완성됐으므로, index.js에서 호출만 해주면 된다.

```jsx
function loadFeedbackHandler() {
  fetch('/api/feedback')
    .then(response => response.json())
    .then(data => {
      console.log(data)
    })
}
```

## 사전 랜더링으로 API 호출하기

- 위의 예시는 버튼을 클릭했을 때 API를 호출하는 방식을 구현했다면 이번엔 사전 랜더링으로 API를 호출하는 방식을 구현한다. 사전 랜더링은 NextJS의 핵심이라 할 수 있는 getStaticProps(), getServersideProps()를 통해 구현할 수 있다.
- 다만 한 가지 주의할 점은 getStaticProps(), getServersideProps() 내부에서 직접 fetching을 하면 안된다는 것이다. 대신 앞서 작성한 api 폴더의 로직을 import해서 data를 가져오는 방식을 사용한다.

```jsx
import React from 'react'

import { buildFeedbackPath, extractFeedback } from '../api/feedback'

export default function FeedbackPage({ feedbackItems }) {
  return (
    <ul>
      {feedbackItems.map(item => (
        <li key={item.id}>{item.text}</li>
      ))}
    </ul>
  )
}

export async function getStaticProps() {
  /* 자체 API를 getStaticProps나 getServerSideProps에서 사용하면 안된다.
    대신 api에서 작성한 함수를 import 해온다.
    */
  const filePath = buildFeedbackPath()
  const data = extractFeedback(filePath)
  return {
    props: {
      feedbackItems: data,
    },
  }
}
```

# 동적으로 API Routes 구성하기

- NextJS에서 동적으로 page를 구성할 때와 비슷한 방식으로 동적으로 API Routes를 구성할 수 있다.
- 먼저 api 폴더 안에 동적으로 구성할 페이지의 이름의 파일을 만든다. 가령, id값을 받아와 동적 페이지를 만들고 싶다면 [id].js라는 형태로 작성할 수 있다. 예시에선 feedbackId를 받아와 동적 페이지를 구성하는 것을 보여준다.

```jsx
// /api/[feedbackId].js

import { buildFeedbackPath, extractFeedback } from './feedback'

function handler(req, res) {
  const { feedbackId } = req.query

  const filePath = buildFeedbackPath()
  const feedbackData = extractFeedback(filePath)

  const selectedFeedback = feedbackData.find(
    feedback => feedback.id === feedbackId
  )

  res.status(200).json({
    feedback: selectedFeedback,
  })
}
export default handler
```

- req.query를 사용해 url로 부터 feedbackId 값을 추출해서 변수에 담고, 기존의 데이터 중 id와 일치하는 값만 뽑아서 response로 보내는 로직이다.
- api쪽 로직이 작성됐으면 이제 페이지에서 어떤식으로 request를 보내고 response 데이터를 받아올지를 정의해야 한다.

```jsx
const [feedbackData, setFeedbackData] = useState()
function loadFeedbackHandler(id) {
  fetch(`/api/${id}`)
    .then(res => res.json())
    .then(data => {
      setFeedbackData(data.feedback)
    })
}
```

- 위 함수는 id값을 인자로 받아와 해당 함수가 호출 됐을 때 받아온 데이터를 feedbackData에 세팅하는 로직을 담고 있다.
