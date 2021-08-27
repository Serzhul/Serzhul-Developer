---
title: 우선순위 큐(Priority Queue)
date: 2021-08-26 19:08:04
category: Data Structure
thumbnail: { thumbnailSrc }
draft: false
---

# 우선순위 큐란 무엇인가?

각 요소가 우선순위를 갖고 있는 자료구조를 말한다. 더 높은 우선순위를 갖고 있는 요소들은 낮은 우선순위를 갖고 있는 요소들보다 먼저 사용된다.

## 일반적인 우선순위 큐

- 리스트를 사용해 모든 요소들을 저장한다.
- 가장 높은 우선 순위 요소를 찾기 위해 모든 요소를 순회한다.

## Priority Queue 의 의사코드

- 최소 이진 힙을 작성한다. - 낮은 숫자 일 수록 높은 우선 순위를 갖고 있는 것을 의미한다.
- 각 노드는 val(값)과 priority(우선순위)를 갖는다. 우선 순위를 사용해 heap을 구현한다.
- Enqueue 메소드는 값과 우선 순위를 받으며, 새로운 노드를 생성하고 우선순위에 따라 적절한 위치에 추가한다.
- Dequeue 메소드는 root 요소를 제거하고, 반환하며, 우선순위를 사용해 힘을 재정리한다.

```jsx
class PriorityQueue {
  constructor() {
    this.values = []
  }
  enqueue(val, priority) {
    let newNode = new Node(val, priority)
    this.values.push(newNode)
    this.bubbleUp()
  }

  bubbleUp() {
    var idx = this.values.length - 1

    const element = this.values[idx]

    while (idx > 0) {
      let parentIdx = Math.floor((idx - 1) / 2)
      let parent = this.values[parentIdx]
      if (element.priority >= parent.priority) break
      this.values[parentIdx] = element
      this.values[idx] = parent
      idx = parentIdx
    }
  }

  dequeue() {
    const min = this.values[0]
    const end = this.values.pop()
    if (this.values.length > 0) {
      this.values[0] = end
      // trickle down
      this.sinkDown()
    }
    return min
  }
  sinkDown() {
    let idx = 0
    const length = this.values.length
    const element = this.values[0]
    while (true) {
      let leftChildIdx = 2 * idx + 1
      let rightChildIdx = 2 * idx + 2
      let leftChild, rightChild
      let swap = null

      if (leftChildIdx < length) {
        leftChild = this.values[leftChildIdx]
        if (leftChild.priority < element.priority) {
          swap = leftChildIdx
        }
      }

      if (rightChildIdx < length) {
        rightChild = this.values[rightChildIdx]
        if (
          (swap === null && rightChild.priority < element.priority) ||
          (swap !== null && rightChild.priority < leftChild.priority)
        ) {
          swap = rightChildIdx
        }
      }

      if (swap === null) break
      this.values[idx] = this.values[swap]
      this.values[swap] = element

      idx = swap
    }
  }
}
```

## 이진 힙(Binary Heaps)의 Big O

- 삽입(Insertion) - $O(log(n))$
- 삭제(Removal) - $O(log(n))$

⇒ 단계별로 역으로 올라가면서 부모 노드를 찾기 때문에 $O(log(n))$이 된다.

## 최악의 시나리오

- 힙은 트리랑 달리 무조건 왼쪽 노드부터 채우기 때문에 트리의 최악의 상황 같은 케이스는 나오지 않는다.
- 그러므로 탐색의 경우 - $O(N)$이다.

## 요약

- 이진 힙은 정렬하고, 우선순위 큐 같은 다른 자료 구조를 구현하는 데 매우 유용하다.
- 이진 힙은 최대 이진 힙 혹은 최소 이진 힙에서 부모가 자기 자식보다 크거나 작다.
- 수학을 사용하면 배열을 통해 힙을 구현할 수 있다.
