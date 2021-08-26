---
title: 그래프(Graphs)
date: 2021-08-26 19:08:26
category: Data Structure
thumbnail: { thumbnailSrc }
draft: false
---

## 그래프란 무엇인가?

그래프 자료구조는 유한한 꼭지점 혹은 노드 혹은 점의 집합으로, 순서가 없는 꼭지점의 집합인 `무향 그래프`와 순서가 있는 꼭지점의 집합인 `유향 그래프` 로 나뉜다.

## 그래프의 사용처

- 소셜 네트워크
- 위치 / 매핑 ⇒ 여러 길이 있지만 지도맵에서는 최적화된 경로를 보여줌
- 라우팅 알고리즘
- 계층 구조
- 파일 시스템 최적화 등.

## 그래프의 종류

### 핵심 그래프 용어

- 꼭지점, 정점(Vertex) - a node (각각의 노드를 가리킴)
- 간선(Edge) - 노드와 노드 사이의 연결선
- 가중/비가중(Weighted/Unweighted) - 값들이 두 정점 사이의 거리에 할당 되어 있는지 아닌지에 따라 가중과 비가중으로 구분
- 유향/무향 (Directed/Undirected) - 방향이 두 정점 사이의 거리에 할당되어 있는지 아닌지에 따라 유향과 무향으로 구분

## 무향 그래프(Undirected graph)

대표적인 예 : Facebook

- 연결점 사이에 방향성이 없고, 정점 사이에 간선이 있다. 그래서 연결되어 있으면 어느 방향에서든 접근할 수 있다.

## 유향 그래프(Directed graph)

대표적인 예 : 인스타그램

- 간선에 방향이 할당되어 있어 특정 방향으로만 연결되어 있는 형태의 그래프를 말한다.

## 가중 그래프(Weighted graph)

대표적인 예 :

- 간선에 값이 할당되어 있는 그래프 ⇒ 해당 값이 정보가 된다.
- 비가중 그래프는 반대로 값이 할당되어 있지 않다.

## 그래프의 표현 방식

### 인접 행렬 (Adjacency matrix)

- 2차원 배열을 활용해서,행렬처럼 구현함

[https://wikimedia.org/api/rest_v1/media/math/render/svg/a773011024de5e3cbe8da03e97c79e1fe3101937](https://wikimedia.org/api/rest_v1/media/math/render/svg/a773011024de5e3cbe8da03e97c79e1fe3101937)

### 인접 리스트 (Adjacency list)

- 2차원 배열을 이용해서 undirected graph 구현
- Hash table 혹은, key-value 자료구조를 통해 구현할수도 있음

![https://media.geeksforgeeks.org/wp-content/uploads/20200205133335/Directed-Graph.jpg](https://media.geeksforgeeks.org/wp-content/uploads/20200205133335/Directed-Graph.jpg)

## 인접 행렬과 인접 리스트의 BIG O 비교

### 표현 방식

`|V| - number of vertices`

`|E| - number of edges`

### 인접 리스트 (Adjacency List)

- 정점 추가(Add vertex) ⇒ $O(1)$
- 간선 추가(Add Edge) ⇒ $O(1)$
- 정점 제거(Remove Vertex) ⇒ $O(|V| + |E|)$
- 간선 제거(Remove Edge) ⇒ $O(|E|)$
- 쿼리(Query) ⇒ $O(|V| + |E|)$
- 저장(Storage) ⇒ $O(|V| + |E|)$ : vertices + edges로 계산한다.

- **산재한 그래프에서는 더 적은 공간을 사용한다.**
- **모든 간선을 더 빠르게 순회할 수 있다.**
- **특정 간선을 찾는데는 조금 더 느릴 수 있다.**

### 인접 행렬(Adjacency Matrix)

- 정점 추가(Add vertex) ⇒ $O(|V^2|)$
- 간선 추가(Add Edge) ⇒ $O(1)$
- 정점 제거(Remove Vertex) ⇒ $O(|V^2|)$
- 간선 제거(Remove Edge) ⇒ $O(1)$
- 쿼리(Query) ⇒ $O(1)$: 특정 간선을 파악할 때 속도가 빠르다.
- 저장(Storage) ⇒ $O(|V^2|)$ : 정점이 많아질 수록 그만큼 배수로 증가한다.

- **산재한 그래프에서 더 많은 공간을 사용한다.**
- **모든 간선을 순회하는데 더 느리다.**
- **특정 간선을 찾는데 더 빠르다.**

- 대부분 현실에서의 데이터는 산재한 그래프이거나 크기가 큰 그래프이기 때문에 `인접 리스트`를 활용한다.

## 그래프 클래스 (Graph Class)

```jsx
class Graph {
  constructor() {
    this.adjacencyList = {}
  }
}
```

## 정점 추가(Adding A Vertex)

- addVertex라는 메소드를 작성하여 정점의 이름을 인자로 받는다.
- 정점의 이름을 인자로 받아 그 값을 빈 배열에 추가하고, 이를 인접 리스트의 키로 추가한다.

```jsx
// Example
// g.addVertex("Tokyo") => { "Tokyo" : [] }

	addVertex(vertex) {
	   this.adjacencyList[vertex] = [];
	}
```

## 간선 추가(Adding an edge)

- vertex1, vertex2라는 두 개의 인자를 받는다.
- 인접 리스트에서 vertex1의 키를 찾고 vertex2를 배열에 push한다.
- 인접 리스트에서 vertex2의 키를 찾고 vertex1를 배열에 push한다.

```jsx
// Example
/* { "Tokyo" : [],
			"Dallas" : [],
			"Aspen" : []
		}
*/

addEgde(v1, v2) {
	    this.adjacencyList[v1].push(v2);
	    this.adjacencyList[v2].push(v1);
	}
```

## 간선 제거(Removing an edge)

- vertex1, vertex2라는 두 개의 인자를 받는다.
- 인접 리스트에서 vertex2를 포함하지 않는 vertex1의 키를 찾아 재할당한다.
- 인접 리스트에서 vertex1를 포함하지 않는 vertex2의 키를 찾아 재할당한다.

```jsx
removeEdge(vertex1, vertex2) {
	    this.adjacencyList[vertex1] = this.adjacencyList[vertex1].filter(
            v => v !== vertex2
	    );
	    this.adjacencyList[vertex2] = this.adjacencyList[vertex2].filter(
            v => v !== vertex1
	    );
	}
```

## 정점 제거(Removing a vertex)

- 삭제하고자 하는 정점을 인자로 받는다.
- 인접 리스트에 해당 정점을 찾을 ㄷ때까지 순회한다.
- 루핑 중 removeEdge라는 함수로 해당 정점을 인자로 받아 인접 리스트에서 값을 삭제하도록 호출한다.
- 인접 리스트에서 해당 정점의 키를 제거한다.

```jsx
removeVertex(vertex) {
	    while(this.adjacencyList[vertex].length) {
	        const adjacenctVertex = this.adjacencyList[vertex].pop();

	        this.removeEdge(vertex, adjacenctVertex);

	    }
	    delete this.adjacencyList[vetex];
	}
```
