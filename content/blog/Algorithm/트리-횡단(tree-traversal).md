---
title: 트리 횡단(Tree Traversal)
date: 2021-08-26 19:08:26
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

트리 횡단이란, 트리 내의 모든 노드를 한번씩 방문하는 것을 말한다. 트리 횡단 방식에는 크게 2가지가 있다. 바로 너비 우선 탐색(Breadth-First Search), 깊이 우선 탐색(Depth-First Search)이다.

## 너비 우선 탐색(Breadth-First Search)

단계 - 반복 방식

- 큐와(배열도 가능하다), 방문했던 노드의 값들을 저장하기 위한 변수(배열)를 만든다.
- 큐에 root 노드를 놓는다.
- 큐에 값이 있는 동안 계속 looping한다.
  - 큐에서 dequeue하고, 노드에 저장되어 있는 노드의 값을 변수에 저장하고 push한다.
  - dequeue한 노드에 left 속성이 있으면 queue에 추가한다.
  - dequeue한 노드에 right 속성이 있으면 queue에 추가한다.

```jsx
BFS() {
        var node = this.root,
            data = [],
            queue = [];

        queue.push(this.root);
        while(queue.length) {
            node = queue.shift();
            data.push(node.value);
            if(node.left) queue.push(node.left);
            if(node.right) queue.push(node.right);
        }
        return data;
    }
```

# 깊이 우선 탐색(Depth-First Search)

- PreOrder(전위), PostOrder(후위), InOrder(중위)가 있는데, 이는 어떤 순서로 탐색할 것인지에 대한 방법의 차이일 뿐 로직은 거의 비슷하다. 다만 순서가 조금씩 달라진다.

## PreOrder (전위)

의사 코드 단계 - 재귀방식

- 방문한 노드 값들을 저장하기 위한 변수를 만든다.
- current라는 변수를 만들어 BST의 root를 저장한다.
- 노드를 인자로 받는 도움 함수를 만든다.
  - 값들을 저장하는 변수에 노드의 값을 push한다.
  - 노드가 left 속성이 있으면, 노드의 left 속성값을 인자로 도움 함수를 호출한다.
  - 노드가 right 속성이 있으면, 노드의 right 속성값을 인자로 도움 함수를 호출한다.
- 도움 함수를 현재 변수로 호출한다.
- 배열의 값들을 반환한다.

```jsx
DFSPreOrder() {
        var data = [];
        var current = this.root;

        function traverse(node) {
            data.push(node.value);
            if(node.left) {
                traverse(node.left);
            }
            if(node.right){
                traverse(node.right);
            }
        }
        traverse(current);

        return data;

    }
```

## 후위(PostOrder) - 끝 값을 먼저 찾고 그 다음에 나머지를 찾는 방식

- root부터 시작하지만 가장 왼쪽 끝 값과, 가장 오른쪽 끝 값을 조회한 후에 시작한다.
- root는 가장 마지막에 방문하게 된다.
- 의사코드는 위와 동일

```jsx
DFSPostOrder() {
        var data = [];
        var current = this.root;

        function traverse(node) {
            if(node.left) {
                traverse(node.left);
            }
            if(node.right){
                traverse(node.right);
            }
            data.push(node.value);
        }
        traverse(current);

        return data;

    }
```

## 중위(InOrder)

- 중위는 root를 가장 마지막에 탐색하기 때문에 left의 끝값을 먼저 찾고, 그 다음에 right, 마지막을 root를 찾는다.

```jsx
DFSInOrder() {
	var data = [];
	function traverse(node) {
				if(node.left) traverse(node.left);
				data.push(node.value);
				if(node.left) traverse(node.right);
	}
		traverse(this.root);
		return data;
	}
}
```

## BFS VS DFS 어느 쪽이 더 우수한가?

- DFS가 공간 복잡도 측면에선 더 유리함
- 시간복잡도는 같다.

## 요약

- 트리는 root와 child 노드들을 포함하고 있는 비선형 자료 구조이다.
- 이진 트리는 어떤 타입이든지 값으로 가질 수 있지만, 부모 당 최대 2개의 자식을 가질 수 있다.
- 이진 탐색 트리는 이진 트리의 좀 더 특별한 버전으로, 모든 노드의 왼쪽은 부모보다 작고, 오른쪽은 부모보다 크다는 특징을 갖는다.
- BFS와 DFS 탐색방식을 통해 트리를 탐색할 수 있다.
