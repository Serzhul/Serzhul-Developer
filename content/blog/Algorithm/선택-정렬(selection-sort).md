---
title: 선택 정렬(Selection Sort)
date: 2021-08-26 18:08:22
category: algorithm
thumbnail: { thumbnailSrc }
draft: false
---

### 선택 정렬은 버블 정렬과 비슷하지만, 큰값들을 정렬한 위치에 놓는 대신 작은 값들을 정렬한 위치에 놓는다.

### 선택 정렬 과정 예시

```jsx
;[5, 3, 4, 1, 2] // 인덱스 0부터 시작하며 가장 작은 값을 현재 인덱스와 스왑하는 방식으로 진행함
// 1 [ 1, 3, 4, 5, 2 ]
// 2 [ 1, 2, 4, 5, 3 ] => 인덱스 0은 정렬된 값이므로 인덱스 1과 가장 작은 값을 또 스왑함.
// 반복...
```

## 선택 정렬 의사코드

- 첫번째 요소를 여태까지 확인한 가장 작은 값에 저장한다.
- 이 값 보다 더 작은 숫자를 찾을때까지 배열의 다음 요소와 비교한다.
- 만약 더 작은 숫자를 찾았으면 그 숫자가 새로운 최솟값이 되고 배열이 끝날때까지 반복된다.
- 만약 최솟값이 처음에 시작할 때 저장한 값이 아니라면 두 값을 서로 스왑한다.
- 배열이 정렬될 때까지 이 과정을 반복한다.

```jsx
// ES5

function selectionSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    let lowest = i

    for (let j = i + 1; j < arr.length; j++) {
      if (arr[j] < arr[lowest]) {
        lowest = j
      }
      if (i !== lowest) {
        // 자기 자신이 min이면 스왑할 필요가 없으므로 조건 추가해줌
        let temp = arr[i]
        arr[i] = arr[lowest]
        arr[lowest] = temp
      }
    }
  }
  return arr
}

// ECMA 2015

function selectionSort(arr) {
  const swap = (arr, idx1, idx2) =>
    ([arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]])

  for (let i = 0; i < arr.length; i++) {
    let lowest = i
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[lowest] > arr[j]) {
        lowest = j
      }
    }
    if (i !== lowest) swap(arr, i, lowest)
  }

  return arr
}
```

# 선택 정렬의 시간 복잡도

- $O(N^2)$
