---
title: 그래프 횡단(Graph Traversal)
date: 2021-08-26 19:08:15
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

# 그래프 횡단의 사용 예(Graph Traversal Uses)

- P2P 네트워킹 (Peer to peer networking)
- 웹 크롤러(Web crawlers)
- facebook 친구 추천
- 최단경로 문제들
  - GPS 네비게이션
  - 미로 찾기
  - AI (shortest path to win the game)

## 깊이 우선 횡단(Depth First Traversal)

- 트리와 달리 그래프에서는 루트가 없기 때문에 임의로 시작점을 정해놓고 탐색을 시작한다.
- 먼저 시작점의 이웃점을 방문한다. (edge가 있는) 이때 breadth first와 다른 점은, bread first는 시작점의 모든 이웃점들을 한번씩 방문하고 그 다음 단계로 넘어간다면 depth는 하나의 시잠점을 정했으면 한 방향으로만 계속 탐색한다.
- 자신이 탐색했던 vertex는 기억해야한다.

# DFS 의사 코드

## 재귀 방식(Recursive)

```jsx
DFS (vertex) :
	if vertex is empty
			return (this is base case)
	add vertex to results list
	mark vertex as visited
	for each neighbor in vertex's neighbors:
		if neighbor is not visited:
			recursively call DFS on neighbor
```

- 방문했던 값 ⇒ true 등으로 표시.
- 이웃한 정점 ⇒ 방문했음으로 표시.

## 깊이 우선 횡단(Depth First Traversal) - 재귀 방식 의사코드

- 시작 노드를 인자로 받는다.
- 마지막 결괏값을 저장할 리스트를 만들고 가장 마지막에 이를 반환한다.
- 방문했던 정점들을 저장할 객체를 생성한다.
- 정점을 인자로 받는 도움 함수를 만든다.
  - 도움 함수는 정점이 빈 값이면 먼저 반환한다.
  - 또한 방문했던 정점을 모아놓은 객체에 값을 넣고, 결괏값 배열에 push한다.
- 인접 리스트에 있는 모든 정점들의 값을 순회한다.
- 한번도 방문한 적이 없는 값이면 해당 정점에 대해 재귀적으로 함수를 호출한다.

```jsx
DFS(start) {
	    const result = [];
	    const visited = {};
	    const adjacencyList = this.adjacencyList;

	    (function dfs(vertex) {
	        if(!vertex) return null;
	        visited[vertex] = true;
	        result.push(vertex);
	        adjacencyList[vertex].forEach(neighbor => {
	            if(!visited[neighbor]) {
	                return dfs(neighbor)
	            }
	        })
	    })(start);

	    return result;

	}
```

### 깊이 우선 횡단(Depth First Traversal) - 반복 방식(Iteratively)

```jsx
/* DFS-iterative(start):
		let S be a stack
		S.push(start)
		while S is not empty
				vertex = S.pop()
				if vertex is not labeled as discovered:
						visit vertex (add to result list)
						label vertex as discovered
						for each of vertex's neighbors, N do
								S.push(N)
*/
```

## 깊이 우선 횡단(Depth First Traversal) - 반복 방식 의사코드

- 시작 노드를 인자로 받는다.
- 리스트 또는 배열을 사용해 정점을 추적하는데 도움을 줄 스택을 만든다.
- 마지막 결괏값을 저장할 리스트를 만들고, 가장 마지막에 이를 반환한다.
- 방문했던 정점들을 저장할 객체를 만든다.
- 스택에 시작 정점을 추가하고, 방문 했음을 체크한다.
- 스택에 무언가 들어있으면 계속 순회한다.
  - 스택에서 그 다음 정점을 pop한다.
  - 만약 정점에 아직 방문하지 않았다면:
    - 방문 했음을 체크한다.
    - 결과 리스트에 추가한다.
    - 모든 이웃들을 스택에 push한다.
- 결과 배열을 반환한다.

```jsx
DFS(start) {
        const stack = [start];
        const result = [];
        const visited = {};
        let currentVertex;

        visited[start] = true;
        while(stack.length > 0) {
            currentVertex = stack.pop();
            result.push(currentVertex);

            this.adjacencyList[currentVertex].forEach(neighbor => {
                if(!visited[neighbor]) {
                    visited[neighbor] = true;
                    stack.push(neighbor)
                }
            });
        }
        return result;
    }
```

## 너비 우선 그래프 횡단 Breadth First Graph Traversal)

## 너비 우선 그래프 횡단Bread First Graph Traversal) 의사코드

- 시작 정점을 인자로 받는다.
- 시작 정점을 위치시킬 큐(배열도 가능하다)를 만든다.
- 방문했던 노드를 저장하기 위한 배열을 만든다.
- 방문했던 노드를 저장하기 위한 객체를 만든다.
- 시작 정점을 방문 했음으로 마킹한다.
- 큐에 무언가 들어있으면 계속 순회한다.
- 큐에서 첫 번째 정점을 제거하고 방문했던 노드를 담는 배열에 push한다.
- 방문했던 인접 리스트에 있는 정점들을 방문하는 것을 반복한다.
- 정점이 방문했던 노드들의 객체에 들어있지 않으면, 방문 했음에 표시하고 enqueue한다.
- 루핑을 끝내고 나면 방문 했던 노드들의 배열을 반환한다.

```jsx
BFS(start) {
        const queue = [start];
        const result = [];
        const visited = {};
        const adjacencyList = this.adjacencyList;
        let currentVertex;

        visited[start] = true;

        while(queue.length) {
            currentVertex = queue.shift();
            result.push(currentVertex);

            this.adjacencyList[currentVertex].forEach(neighbor => {
                if(!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.push(neighbor);
                }
            })
        }
        return result;

    }
```
