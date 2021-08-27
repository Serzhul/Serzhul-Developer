---
title: 병합 정렬(Merge Sort)
date: 2021-08-26 19:08:35
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

### 중급 정렬 알고리즘

- 병합 정렬(Merge Sort)
- 퀵 정렬(Quick Sort)
- 기수 정렬(Radix Sort)

기본 정렬 알고리즘들(Bubble, Insertion, Selection sort)은 소규모 크기용 정렬이다. 그도 그럴 것이, 시간 복잡도가 $O(N^2)$ 이기 때문에 효율성이 많이 떨어지기 때문이다.

그러나 병합 정렬을 비롯한 중급 정렬 알고리즘들(Merge, Quick, Radix sort)은 시간 복잡도가 $O(n * logn)$ 이기 때문에 훨씬 효율적이라 할 수 있다.

⇒ 단 기본 정렬 알고리즘들은 직관적이고 이해하기에 큰 어려움이 없지만, 중급 정렬 알고리즘은 조금 더 복잡하다. 즉 효율성과 복잡성은 상쇄관계에 있다. 좀 더 효율적인 알고리즘은 더 복잡하고, 일반적으로 이해하기에 시간이 더 걸린다.

# 병합 정렬(Merge Sort)

병합 정렬은 크게 두가지 파트로 나눈다. ⇒ 병합과 그리고 정렬이다.

배열을 더 작은 배열로 분리해서 정렬하고, 이를 새로운 값으로 합쳐 정렬된 배열을 만들어 내는 알고리즘이다.

## 배열 병합

- 병합 정렬을 구현하기 위해 두개의 정렬된 배열을 병합하는 기능을 가진 함수를 구현하는 것이 도움이 된다.
- 정렬되어 있는 2개의 배열이 주어지면, 이 함수는 모든 요소들이 두개의 인풋 배열의 요소들로 이루어져 있고 정렬된 새로운 배열을 만들어야 한다.
- 이 함수는 $O(n+m)$번 시행 되어야 하며, $O(n+m)$ 만큼의공간을 차지하고, 인풋으로 들어온 파라미터를 수정하면 안된다.

## 배열 병합 의사코드

- 빈 배열을 만들고, 인풋 배열중 가장 작은 값들을 찾는다.
- 확인하지 않은 값이 있다면 반복
  - 만약 첫번째 배열의 값이 두번째 배열의 값보다 작다면, 첫번째 배열의 값을 결과값에 넣고 첫번째 배열의 다음 값으로 넘어간다.
  - 만약 첫번째 배열의 값이 두번째 배열의 값보다 크면, 두번째 배열의 값을 결과값에 넣고 두번째 배열의 다음 값을 ㅗ넘긴다.
  - 하나의 배열을 다 사용하고 나면, 다른 배열의 남은 값들을 push한다.

## 배열 병합 구현

```jsx
function merge(arr1, arr2) {
  let results = []
  let i = 0
  let j = 0

  while (i < arr1.length && j < arr2.length) {
    if (arr2[j] > arr1[i]) {
      results.push(arr1[i])
      i++
    } else {
      results.push(arr2[j])
      j++
    }
  }
  while (i < arr1.length) {
    results.push(arr1[i])
    i++
  }
  while (j < arr2.length) {
    results.push(arr2[j])
    j++
  }

  return results
}
```

- merge는 2개의 배열을 인자로 받는 함수이다.
- 먼저 빈 배열(results)을 생성하고 각각의 배열의 index값을 주기 위해 i,j로 선언한다.
- while문을 돌리는데 두 배열을 비교해 짧은 쪽의 인덱스가 끝날때까지 루프를 돈다.
- 이때, 두개의 배열 값을 비교해 작은 쪽을 빈 배열에 추가한다.
- while문이 끝났다는 것은 둘 중에 작은 배열쪽 기준으로 루프가 끝났다는 의미이므로, 두 배열중 남은 배열의 값들은 그냥 추가만 하면 된다.
- 다 돌았는지 안돌았는지 비교하는 현재 인덱스(i, j)가 각각의 배열 길이보다 짧은지 여부 ⇒ while로 체크
- 마지막엔 완성한 배열(results)을 return

## 병합정렬 의사코드

- 비어있거나, 요소가 하나가 남을 때까지 배열을 반으로 나눈다.
- 더 작게 정렬된 배열이 있다면 다른 정렬된 배열에 합친다.
- 두 배열이 합치면 합쳐진 배열을 반환한다.

```jsx
function mergeSort(arr) {
  if (arr.length <= 1) return arr

  let mid = Math.floor(arr.length / 2)
  let left = mergeSort(arr.slice(0, mid))
  let right = mergeSort(arr.slice(mid))

  return merge(left, right)
}

function merge(arr1, arr2) {
  let results = []
  let i = 0
  let j = 0

  while (i < arr1.length && j < arr2.length) {
    if (arr2[j] > arr1[i]) {
      results.push(arr1[i])
      i++
    } else {
      results.push(arr2[j])
      j++
    }
  }
  while (i < arr1.length) {
    results.push(arr1[i])
    i++
  }
  while (j < arr2.length) {
    results.push(arr2[j])
    j++
  }
  return results
}
```

## 병합정렬의 Big O

- 시간 복잡도(Best) ⇒ $O(n log n)$ ⇒ 2로 나눠서 반복 할 때마다 그 개수가 반으로 줄어듬으로 $log(n)$이 나오고, 또한 그 나눈 값들을 비교해야되기 때문에 배열의 개수(n)만큼 반복하기 때문에 $nlog(n)$이 된다.
- 시간 복잡도(Average) ⇒ $O(n log n)$
- 시간 복잡도(Worst) ⇒ $O(n log n)$
- 공간 복잡도 ⇒ $O(N)$
