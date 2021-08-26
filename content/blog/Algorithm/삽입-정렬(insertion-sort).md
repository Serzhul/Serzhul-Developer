---
title: 삽입 정렬(Insertion Sort)
date: 2021-08-26 18:08:53
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

삽입 정렬은 왼쪽 절반 부분은 항상 정렬된 상태로 만들고 점진적으로 더 크게 만들면서 정렬을 완성한다.

# 삽입 정렬 의사코드

- 두번째 요소를 선택하고, 그 전 요소랑 비교해서 필요하면 스왑함.
- 그 다음 요소는 자신의 왼쪽(정렬된 portion)과 비교해 반복하며 필요한 위치에 들어가도록 삽입함.
- 끝날때까지 반복

```jsx
function insertionSort(arr) {
  for (var i = 1; i < arr.length; i++) {
    // 인덱스 0과 비교하려면 1부터 시작해야한다.
    let currentVal = arr[i]
    for (var j = i - 1; j >= 0 && arr[j] > currentVal; j--) {
      // 마지막 element부터 시작해서, 그 전 element와 크기를 비교후, 자기자신보다 크면 그 값을 복사해 자기 자신의 자리에 넣는다.
      // 그런식으로 반복하다가 currentVal가 작아지는 순간까지 반복하게되면 정렬이 완성된다.
      arr[j + 1] = arr[j]
    }
    arr[j + 1] = currentVal
  }
  return arr
}

// console.log로 찍어보면서 연습
```

# 삽입 정렬의 시간 복잡도

- $O(N^2)$
- 거의 정렬되어 있는 경우라면, $O(1)$
- 온라인에서 데이터가 라인이나, 스트리밍으로 들어올 때 정렬하는 알고리즘으로서 유용하다.
