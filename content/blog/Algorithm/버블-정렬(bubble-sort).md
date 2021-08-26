---
title: 버블 정렬(Bubble Sort)
date: 2021-08-26 18:08:44
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

$cf)$ e.g. ⇒ e.g. 는 라틴어 exempli gratia의 약자로, for example 혹은 for the sake of example을 의미

i.e. ⇒ i.e.는 라틴어 id est의 약자로, that is to say 혹은 in other words에 해당한다.

# 정렬 알고리즘의 기초

- 버블 정렬(Bubble Sort)
- 선택 정렬(Selection Sort)
- 삽입 정렬(Insertion Sort)

# 정렬이란 무엇인가?

정렬이란 콜렉션(예를 들면 배열 같은)에 있는 아이템들을 재정렬하는 과정을 의미하며, 아이템이 특정 종류의 순서를 따른다.

> 예시

- 가장 작은 수 부터 큰 숫자로 정렬하기
- 알파벳 순으로 이름 정렬하기
- 영화 출시일순으로 정렬하기
- 영화 수익순으로 정렬하기

### 정렬을 배워야 하는 이유?

- 정렬은 흔히 볼 수 있는 작업으로, 어떻게 동작하는지 알아둘 필요가 있다.
- 무언가를 정렬하는 방법은 무척 다양하지만, 각각의 방법들은 그 나름의 장단점이 있기 때문.

### 자바스크립트는 정렬 메소드가 존재하지만...

- 일반적으로 사용하는 정렬 메소드는 문자열의 유니코드와 관련되어있어, 숫자 정렬에선 다른 방식을 사용해야한다.

### 자바스크립트는 어떻게 정렬하는가?

- 내장되어있는 sort 메소드는 여러 비교자 함수를 인자로 받는다.
- 이 비교자 함수를 통해 자바스크립트에게 어떤 방식으로 정렬시키고 싶은지 알려줄 수 있다.
- 비교자는 마치 요소의 한쌍처럼 보인다. (가장 많이 사용하는 방식은 a, b)

  반환하는 값을 통해 정렬 순서를 정의하게 된다.

  - 만약 반환되는 값이 음수라면, a 다음 b가 오는 방식으로 정렬된다.
  - 만약 반환되는 값이 양수라면, b 다음 a가 오는 방식으로 정렬된다.

  ```jsx
  // 일반적인 자바스크립트 sort 메소드 사용방식

  Array.sort((a, b) => a - b)
  ```

# 버블 정렬: 개관

버블 정렬이란, 마치 거품이 떠오르듯이 가장 큰 값을 계속해서 올리는 방식으로 정렬하는 것을 말한다.

- 대부분의 정렬 알고리즘은 스와핑 기능을 포함한다. ⇒ 스와핑이 정렬의 핵심기능이기 때문!

```jsx
// Examples

// ES5
function swap(arr, idx1, idx2) {
  var temp = arr[idx1]
  arr[idx1] = arr[idx2]
  arr[idx2] = temp
}

// ES2015
const swap = (arr, idx1, idx2) => {
  ;[arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]]
}
```

## 버블 정렬 의사코드

- 배열의 끝 값을 i라는 변수로 선언하여, 배열의 처음으로 루핑하게 된다.
- 내부 loop는 j라는 변수로 배열의 처음부터 시작해 i -1까지 루핑하게 된다.
- 만약 arr[j]가 arr[i+1]보다 크다면, 두 값을 swap한다.
- 정렬된 배열을 반환한다.

```jsx
function bubbleSort(arr) {
  for (let i = arr.length; i > 0; i--) {
    // optimization을 위해 i 값을 줄이고
    for (let j = 0; j < i - 1; j++) {
      // j값도 i 값에 따라 줄어드는 방식으로 변경
      console.log(arr)
      if (arr[j] > arr[j + 1]) {
        // SWAP!
        let temp = arr[j]
        arr[j] = arr[j + 1]
        arr[j + 1] = temp
      }
    }
  }

  return arr
}
```

## 버블 정렬 최적화(BubbleSort Optimization)

- 버블 정렬은 정렬이 완료됐음에도, 배열의 길이에 따라 끝까지 비교하게 되므로 쓸데 없는 시간이 낭비가 된다. 따라서 이를 최적화 하기 위해 다음과 같은 작업이 필요하다.

- 스왑을 안한다는 것은 이미 정렬이 끝난다는 의미이므로, 끝낸다.

```jsx
function bubbleSort(arr) {
  let noSwaps // swap 여부를 체크하는 변수 추가
  for (let i = arr.length; i > 0; i--) {
    noSwaps = true // looping 시작할때 noSwap값 초기화
    for (let j = 0; j < i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        // SWAP!
        let temp = arr[j]
        arr[j] = arr[j + 1]
        arr[j + 1] = temp
        noSwaps = false // Swap이 있었으므로 false로 값 변경
      }
    }
    if (noSwaps) break
  }

  return arr
}
```

## 버블 정렬의 시간 복잡도

- 일반적으로 O(n2) ⇒ 이중 루프가 사용되므로
