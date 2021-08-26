---
title: 이진 힙(Binary Heaps)
date: 2021-08-26 19:08:26
category: Data Structure
thumbnail: { thumbnailSrc }
draft: false
---

## 이진 힙(Binary Heaps)이란?

이진 탐색 트리와 매우 유사하지만 몇가지 다른 규칙을 갖는다.

- 최대 이진 힙(MaxBinaryHeap)에서는 부모 노드가 항상 자식 노드들보다 크다.
- 최소 이진 힙(MinBinaryHeap)에서는 부모 노드가 항상 자식 노드들보다 작다.

## 최대 이진 힙(Max Binary Heap)

- 부모 당 최대 2개의 자식 노드를 갖는다.
- 각 부모의 값은 항상 자식 노드들보다 크다.
- 최대 이진 힙에서 부모는 자식들보다 크지만, 형제 노드 사이엔 그 규칙이 보장되지 않는다.
- 이진 힙은 규칙에 의해 왼쪽 자식 부터 빈 노드 없이 채워진다.(자식 노드는 부모노드 보다 항상 작아야 하므로)
- 최소 이진 힙 역시 최대 이진 힙과 반대의 규칙대로 채워지게 된다.

## 이진 힙을 배워야 하는 이유

이진 힙은 우선순위 큐를 구현할 때 사용되는데, 우선순위 큐는 보편적으로 자주 사용되는 자료구조이기 때문이다. 특히 그래프 횡단 알고리즘 등에 사용된다.

- 이진 힙을 구현하는 방법으로는 list나 array를 활용하는 방법이 있다.
- 또한 n의 길이를 갖고 있는 배열에서 left 자식 값은 항상 2n+1에, right 자식은 항상 2n+2에 저장된다.

만약 자식 노드를 알고 부모 노드를 찾고 있다면 부모 노드는 `index Math.floor((n-1)/2)` 에 위치한다.

## 이진 힙의 class 정의

```jsx
/* Class Name:
		MaxBinaryHeap

	Properties:
		values = [] */

Class MaxBinaryHeap {
	constructor() {
		this.values = [];
	}
}
```

## Insert 메소드의 의사코드

- 힙의 values 속성에 값을 push한다.
- Bubble Up(도움 함수):
  - index라는 변수를 만들어 values 속성의 길이 -1를 저장한다.
  - parentIndex라는 변수를 만들어 (index-1)/2 값을 저장한다.
  - values 요소의 parentIndex가 values 요소의 child index보다 작아질 때까지 반복한다.
    - values 요소의 parentIndex 값과 child index 값을 스왑한다.
    - index를 parentIndex에 할당하고 처음부터 반복한다.

```jsx
class MaxBinaryHeap {
  constructor() {
    this.values = [41, 39, 33, 18, 27, 12]
  }

  insert(element) {
    this.values.push(element)
    this.bubbleUp()
  }

  bubbleUp() {
    var idx = this.values.length - 1

    const element = this.values[idx]

    while (idx > 0) {
      let parentIdx = Math.floor((idx - 1) / 2)
      let parent = this.values[parentIdx]
      if (element <= parent) break
      this.values[parentIdx] = element
      this.values[idx] = parent
      idx = parentIdx
    }
  }
}
```

## 힙에서의 삭제

- root를 삭제한다.
- 가장 최근에 추가된 것으로 대체한다.
- Sink down

## Sink Down이란?

힙에서 root를 삭제하는 과정(최대 힙에서 최댓값을, 최소 힙에서 최솟값을 효과적으로 추출하는 것)과 다운 힙에서 속성들을 저장하는 것을 말한다.(다운 힙은 bubble-down, percolate-down, sift-down, trickle-down, heapify-down, cascade-down, extract-min/max으로도 불린다.)

## Removing (또는 extractMax)의 의사코드

- values 속성의 첫 번째 값과 마지막 값을 스왑한다.
- values 속성으로 부터 pop하고, 마지막에 그 값을 반환한다.
- 새로운 root가 제 자리에 sink down(최상위 노드에서 하위노드로 내려가는 것)할 수 있도록 다음과 같이 로직을 진행한다.

  - parentIndex는 0부터 시작한다(root)
  - left child의 index를 찾는다. (2\*index + 1) ⇒ 범위를 벗어나지 않는 값
  - right child의 index를 찾는다. (2\*index + 2) ⇒ 범위를 벗어나지 않는 값
  - left 혹은 right child가 더 크다면 스왑한다. 둘 다 보다 크다면 둘 중에 더 큰 값과 스왑한다.
  - 스왑한 child index가 새로운 parent index가 된다.
  - child가 크지 않을 때까지 looping과 swapping을 반복한다.
  - old root를 반환한다.

  ```jsx
  sinkDown() {
    let idx = 0;
    const length = this.values.length;
    const element = this.values[0];

    while(true) {
      let leftChildIdx = 2 * idx + 1;
      let rightChildIdx = 2* idx + 2;
      let leftChild, rightChild;
      let swap = null;

      if(leftChildIdx < length) {
        leftChild = this.values[leftChildIdx];
        if(leftChild > element) {
          swap = leftChildIdx;
        }
      }

      if(rightChildIdx < length) {
        rightChild = this.values[rightChildIdx];
        if(swap === null && rightChild > element ||
          swap !== null && rightChild > leftChild) {
          swap = rightChildIdx;
        }
      }

      if(swap === null) break;
      this.values[idx] = this.values[swap];
      this.values[swap] = element;
    }

  }
  ```
