---
title: 퀵 정렬(Quick Sort)
date: 2021-08-26 19:08:90
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

퀵 정렬은 병합 정렬과 마찬가지로 0번째, 첫번째 요소를 항상 정렬된 상태로 만든다는 특징을 갖는다.

- 알고리즘 방식을 살펴보면 피벗(pivot)으로 불리는 요소를 하나 선택하여, 피벗을 기준으로 정렬된 배열의 인덱스를 찾는다.
- pivot 포인트를 하나 지정해서, 그 포인트보다 작은 값들을 먼저 구한다. (스왑)
- 작은 값들의 갯수에 따라 피벗 값의 인덱스 위치가 정해지고 재귀를 통해 이를 반복한다.

## 피벗 함수

- 병합 정렬을 구현하기 위해, 피벗 양쪽의 요소들을 정리하는 함수를 구현하는 것이 유용하다.
- 배열이 주어지면, 이 도움 함수는 요소 하나를 피벗으로 설정해야 한다.
- 배열 안의 요소들 중 **피벗보다 작은 모든 값들은 피벗보다 왼쪽으로**, **피벗보다 큰 모든 값들은 피벗보다 오른쪽으로** 재정리되야 한다.
- **이 때, 요소들의 순서는 중요하지 않다.**
- 이 도움 함수는 자기 공간에서 작용한다. 즉, 추가 배열을 만들 필요는 없다.
- 작업이 끝나면, 피벗의 인덱스를 반환한다.

## 피벗 가져오기

- 퀵 정렬의 런타임은 피벗을 어떻게 설정하느냐에 달렸다.
- 피벗은 일반적으로 정렬하고자 하는 데이터의 중간값이 이상적이다.

## 피벗 의사코드

- 세개의 인자를 받는다. (배열, 시작인덱스, 끝인덱스) 시작인덱스의 초기값은 0, 끝인덱스의 초기값은 배열의 length - 1
- 피벗을 배열의 첫번째 값으로 잡는다.
- 현재 피벗 인덱스를 변수로 저장한다.(이 변수로 피벗이 언제 끝나야하는지 추적할 수 있다.)
- 배열의 처음부터 끝까지 루핑한다.
  - 만약 피벗이 현재 요소보다 크다면, 피벗인덱스 변수를 증가시키고 현재 요소와 피벗인덱스의 요소랑 스왑한다.
- 첫 요소와 피벗 인덱스의 요소를 스왑한다.
- 피벗 인덱스를 반환한다.

```jsx
function pivot(arr, start = 0, end = arr.length + 1) {
  //     function swap(array, i, j) {
  //         var temp = array[i];
  //         array[i] = array[j];
  //         array[j] = temp;
  //     }

  const swap = (arr, idx1, idx2) => {
    ;[arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]]
  }

  var pivot = arr[start]
  var swapIdx = start

  for (var i = start + 1; i < arr.length; i++) {
    if (pivot > arr[i]) {
      swapIdx++
      swap(arr, swapIdx, i)
    }
  }

  swap(arr, start, swapIdx)
  return swapIdx
}

pivot([4, 8, 2, 1, 5, 7, 6, 3])
// 3
```

- pivot은 인덱스 0번 값으로 설정 (다르게 설정할 수도 있지만 편의를 위해)
- swap 함수를 구현
- swapIndex 역시 초깃값은 0으로 설정
- for문을 통해 루프를 타는데, pivot은 비교값이 아니므로 start+1부터 루프타도록 설정
- 만약 피벗보다 작은 값이면, swapIdx는 늘어남(피벗 왼쪽에 값들이 와야하므로)
- 그리고 swapIdx에 현재 index의 값이 와야하므로 스왑함. ⇒ 반복
- 마지막으로 pivot을 제자리에 돌려놓기 위해 start와 swapIdx를 스왑함

```jsx
function quickSort(arr, left = 0, right = arr.length - 1) {
  if (left < right) {
    let pivotIndex = pivot(arr, left, right) // 3
    // left
    quickSort(arr, left, pivotIndex - 1)

    // right
    quickSort(arr, pivotIndex + 1, right)
  }

  return arr
}
```

- 위에서 작성한 pivotHelper에 따라 quickSort 함수에선 pivot을 기준으로 왼쪽, 오른쪽을 재귀 형태로 반복하게 된다.
- 먼저 pivotHelper의 결괏값으로 Return되는 pivotIndex를 저장하고, left side는 start가 left, pivotIndex-1이 right인 형태로 반복한다.
- 마찬가지로 rightside역시 leftside는 pivot+1, right는 arr.length - 1인 형태로 반복한다.
- 이렇게 정렬을 계속하다보면 새로운 피벗이 설정되고, 또 그 피벗을 기준으로한 배열이 피벗으로 정렬되는 식으로 반복되게 된다.
- 단, basecase를 지정해야지 무한반복에서 벗어날 수 있는데, 재귀를 하면 할수록 left와 right의 위치가 같아져 결국 동일해지는 것을 알 수 있다. 따라서 조건을 left<right로 줌으로써 재귀의 마지막을 정해준다.

퀵 정렬 Big O

- 시간 복잡도(Best) ⇒ O($nlog(n)$)
- 시간 복잡도Average) ⇒ O($nlog(n)$)
- 시간 복잡도(Worst) ⇒ O($nlog(n)$)
