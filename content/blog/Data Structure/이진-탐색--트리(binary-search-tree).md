---
title: 이진 탐색  트리(Binary Search Tree)
date: 2021-08-26 19:08:50
category: Data Structure
thumbnail: { thumbnailSrc }
draft: false
---

# 트리란 무엇인가?

트리란 노드들이 부모와 자식 관계로 이루어진 자료구조를 말한다.

리스트들이 선형 자료구조였다면, 트리는 비선형 자료구조이며 1개 이상의 다른 경로가 있다.

## 트리 전문용어

- Root - 트리에서 가장 상위 노드를 말함.
- Child - 다른 노드와 직접적으로 연결되어 있는 노드이면서 root와는 떨어져 있는 노드를 말함.
- Parent - Child노드와 반대되는 개념
- Siblings - 같은 부모를 갖고 있는 노드의 그룹
- Left - 자식 노드가 없는 노드를 말함.
- Edge - 노드와 다른 노드 사이의 연결을 말함.

## 트리가 사용되는 예

- HTML DOM
- 네트워크 라우팅
- 추상 구문 트리
- AI
- OS의 폴더구조 등

## 트리의 종류

- 트리
- 이진 트리 ⇒ 이진 트리란, 자식 노드가 최대 2개까지만 가질 수 있는 규칙에 해당하는 트리를 말한다.
- 이진 탐색 트리

## 이진 탐색 트리의 동작 방식

- 모든 부모 노드는 최대 2개의 자식 노드를 갖는다.
- 부모 노드의 왼쪽에 위치하는 노드는 부모 노드보다 항상 작다.
- 부모 노드의 오른쪽에 위치하는 노드는 부모 노드보다 항상 크다.

## The BinarySearchTree Class

```jsx
class Node {
  constructor(value) {
    this.value = value
    this.left = null
    this.right = null
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null
  }
}

var tree = new BinarySearchTree()
tree.root = new Node(10)
tree.root.right = new Node(15)
tree.root.left = new Node(7)
tree.root.left.right = new Node(9)
```

## 노드 삽입(Inserting a node)

두 가지의 접근 방식이 있다. 반복(Iteratively) 혹은 재귀(Recursively)

- 새로운 노드를 만든다.
- root부터 시작한다.
  - root가 없으면 root는 새로 만든 노드가 root가 된다.
  - root가 있으면 새로운 노드의 값이 root노드보다 큰지 작은지를 체크한다.
  - 만약 크다면
    - root의 오른쪽에 node가 있는지 확인한다.
      - 있다면, 해당 노드를 옮기고 이 과정을 반복한다.
      - 없다면, right 속성에 새로운 노드를 추가한다
  - 만약 작다면
    - root의 왼쪽에 node가 있는지 확인한다.
      - 있다면, 해당 노드를 옮기고 이 과정을 반복한다.
      - 없다면, left 속성에 새로운 노드를 추가한다.
- Starting at the root
  - Check if there is root, if not - the root now becomes that new node!
  - If there is a root, check if the value of the new node is greater than or less than the value of the root
  - If it is greater
    - Check to see if there is a node to the right
      - If there is, move to that node and repeat these steps
      - If there is not, add that nodes as the right property
  - If it is less
    - Check to see if there is a node to the left
      - If there is, move to that node and repeat these steps
      - If there is not, add that node as the left property

```jsx
insert(value) {
        var newNode = new Node(value);

        if(this.root === null) {
            this.root = newNode; return this
        }
        else {
            var current = this.root;

            while(true) {
                if(value === current.value) return undefined;
                if(value < current.value) {
                    if(current.left === null) {
                        current.left = newNode;
                        return this;
                    }
                    current = current.left;
                } else {
                    if(current.right === null) {
                        current.right = newNode;
                        return this;
                    }
                     current = current.right;
                }

            }
        }
```

## 이진 탐색 트리에서 노드 찾기(Finding a Node in a BST)

Steps - Iteratively or Recursively

- root에서 시작한다.
  - root가 있는지 확인하고, 없으면 searching을 종료한다.
  - root가 있으면 새로운 노드의 값이 우리가 찾는 값인지 확인한다.
  - 찾았으면 탐색을 종료한다.
  - 아니라면 값이 루트보다 큰 지 작은 지를 판단한다.
  - 크다면
    - 노드의 오른쪽이 있는지 체크한다.
      - 있으면 노드를 옮기고 반복한다.
      - 없으면 탐색을 종료한다.
  - 작다면
    - 노드의 왼쪽이 있는지 체크한다.
      - 있으면 노드를 옮기고 반복한다.
      - 없으면 탐색을 종료한다.

```jsx
find(value) {
        if(this.root === null) {
            return false;
        }
        else {
            var current = this.root;
            var found = false;

            while(current && !found) {
                if(value === current.value) return true;
                if(value < current.value) {
                    if(current.left === null) {
                        return false
                    }
                    current = current.left;
                }
                else {
                    if(current.right === null) {
                        return false
                    }
                    current = current.right;
                }
            }
        }
    }
```

## BST의 Big O

- 삽입(Insertion) - $O(log(n))$
- 탐색(Searching) - $O(log(n))$ ⇒ 평균적인 경우
