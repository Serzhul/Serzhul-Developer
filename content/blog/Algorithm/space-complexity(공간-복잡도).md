---
title: Space Complexity(공간 복잡도)
date: 2021-08-26 18:08:47
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

- 시간 복잡도와는 달리 공간 복잡도는 얼마나 효율적으로 메모리를 할당해서 활용하는가에 중점을 둔다.
- 여기서 예비공간 복잡도(auxiliary space complexity)라는 개념이 나오는데, 알고리즘에서 말하는 공간복잡도는 일반적으로예비공간 복잡도를 기준으로 삼는다.

### JS에서의 공간복잡도

- 대부분의 원시형은 일정한 공간을 사용한다.
- 문자열은 $O(N)$의 공간을 필요로 한다. (여기서 N은 문자열의 길이를 의미한다.)
- 참조 타입은 일반적으로 $O(N)$ 이며, N은 배열의 길이 혹은 객체의 키의 갯수를 의미한다.

### 공간복잡도 측정의 예시

```jsx
function sun(arr) {
  let total = 0
  for (let i = 0; i < arr.length; i++) {
    total += arr[i]
  }
  return total
}

// total, i가 선언되는 변수이므로 공간복잡도는 2이므로 상수인 O(1)이 된다!

function double(arr) {
  let newArr = []
  for (let i = 0; i < arr.length; i++) {
    newArr.push(2 * arr[i])
  }
  return newArr
}
// 인풋데이터의 길이만큼 배열의 길이도 증가하므로 선형증가인 O(n)이 공간복잡도이다.
```
