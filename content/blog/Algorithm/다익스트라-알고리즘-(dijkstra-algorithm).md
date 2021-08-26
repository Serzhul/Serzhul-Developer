---
title: 다익스트라 알고리즘 (Dijkstra's Algorithm)
date: 2021-08-26 19:08:28
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

# 다익스트라 알고리즘이란 무엇인가?

- 그래프, 우선순위 큐, 이진 힙으로 구현할 수 있는 알고리즘이다.
- 알고리즘 중에 가장 유명하고 널리 사용된다.
- 그래프에서 두 개의 정점의 가장 짧은 경로를 찾는 알고리즘이다.
- 주로 A점에서 B점으로 가는 가장 빠른 방법은? 이란 질문에 대한 해답으로 제시된다.

# 다익스트라는 누구인가?

- 에츠허르 다익스트라(Edsger Dijkstra)는 네덜란드 프로그래머이자 물리학자, 수필가로 여러 방면에 뛰어난 인물이다.
- 그는 컴퓨터 공학의 영역이 예술에서 학문 분야로 진보하도록 영향력을 미친 인물이다.
- 그의 많은 발견과 알고리즘들은 요즈음에도 자주 사용된다.

# 다익스트라 알고리즘의 사용되는 곳

- GPS - 가장 빠른 길을 찾기
- 네트워크 라우팅 - 데이터의 경로가 가장 빠른 곳을 찾아줌
- 생물학 - 바이러스가 사람들에게 확산 되는 모델 등에 사용됨
- 항공기 표 - 목적지까지 가장 저렴하게 갈 수 있는 방법을 찾아줌
- 그 외 많은 곳

# 가중 그래프의 작성

```jsx
class WeightedGraph {
  constructor() {
    this.adjacencyList = {}
  }

  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = []
  }
  addEdge(vertex1, vertex2, weight) {
    this.adjacencyList[vertex1].push({ node: vertex2, weight })
    this.adjacencyList[vertex2].push({ node: vertex1, weight })
  }
}
```

# 접근법

1. 새로운 노드를 방문할 때 마다 처음 방문한 곳과 가장 짧은 거리에 있는 노드를 선택한다.
2. 한번 방문했던 노드로 옮기고 나면 그 노드의 이웃을 찾는다.
3. 시작 노드로부터 각각의 이웃노드까지의 거리를 계산하는데 모든 간선을 더함으로써 계산한다.
4. 새로운 총 거리가 기존 총 거리보다 짧다면 새로운 총 거리를 해당 노드에 저장한다.

```jsx
// Example.
/* 1. A => from 0
A-B Infinity => 4 // Previous B : A // Visited = [A]
A-C Infinity => 2 // Previous C : A // Visited = [A, C]
A-C-D Infinity => 4 // Previous D : C // Visited = [A,C]
A-C-F Infinity => 6 // Previous F : C //
A-B-E Infinity => 7 // Previous E : B // Visited = [A,C,B]
A-C-D-F 7 => 5 // Previous F : D, // Visited = [A,C,B,D]

=> draw chart by on your own
```

## 의사 코드

- 시작과 끝 정점을 인자로 받는다.
- distances라는 객체를 만들고, 인접 리스트의 모든 정점을 key로 하고 infinity를 값으로 하도록 세팅한다. 단, 시작 정점은 값을 0으로 한다.
- distances 객체에 값을 세팅한 후, 우선순위 큐에 각 정점의 우선순위를 Infinity로 하여 추가한다. 단 시작 정점은 시작점이므로 priority를 0으로 한다.
- previous라는 객체를 만들어 인접 리스트 내의 각 정점들을 key로 하고, 값을 null로 세팅한다.
- 우선순위 큐에 값이 있으면 게속 순환한다.
  - 우선순위 큐에서 정점을 dequeue한다.
  - 마지막 정점과 dequeue한 정점이 같으면 순환을 종료한다.
  - 그게 아니라면 각 정점의 값을 순환한다.
    - 시작 정점으로부터 해당 정점의 거리를 계산한다.
    - 거리가 현재 distances 객체의 저장되어 있는 거리보다 작다면
      - distances 객체에 더 작은 새로운 거리로 업데이트 한다.
      - previous 객체에 정점을 포함시킨다.
      - 시작 노드로 부터 총 거리에 해당 정점을 enqueue 한다.

```jsx
class PriorityQueue {
  constructor() {
    this.values = []
  }
  enqueue(val, priority) {
    this.values.push({ val, priority })
    this.sort()
  }
  dequeue() {
    return this.values.shift()
  }
  sort() {
    this.values.sort((a, b) => a.priority - b.priority)
  }
}

class WeightedGraph {
  constructor() {
    this.adjacencyList = {}
  }

  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = []
  }
  addEdge(vertex1, vertex2, weight) {
    this.adjacencyList[vertex1].push({ node: vertex2, weight })
    this.adjacencyList[vertex2].push({ node: vertex1, weight })
  }
  Dijkstra(start, finish) {
    const nodes = new PriorityQueue()
    const distances = {}
    const previous = {}
    let path = []
    let smallest

    // build up initial state
    for (let vertex in this.adjacencyList) {
      if (vertex === start) {
        distances[vertex] = 0
        nodes.enqueue(vertex, 0)
      } else {
        distances[vertex] = Infinity
        nodes.enqueue(vertex, Infinity)
      }
      previous[vertex] = null
    }
    // as long as there is something to visit
    while (nodes.values.length) {
      smallest = nodes.dequeue().val
      if (smallest === finish) {
        // We ARE DONE
        // BUILD UP PATH TO RETURN AT END
        while (previous[smallest]) {
          path.push(smallest)
          smallest = previous[smallest]
        }
        break
      }
      if (smallest || distances[smallest] !== Infinity) {
        for (let neighbor in this.adjacencyList[smallest]) {
          // find neighboring node
          let nextNode = this.adjacencyList[smallest][neighbor]
          console.log(nextNode)
          // calculate new distance to neighboring nodes
          let candidate = distances[smallest] + nextNode.weight
          let nextNeighbor = nextNode.node
          if (candidate < distances[nextNeighbor]) {
            // updating new smallest distance to neighbor
            distances[nextNeighbor] = candidate
            // updating previous - How we got to neighbor
            previous[nextNeighbor] = smallest
            // enqueue in priority queue with new priority
            nodes.enqueue(nextNeighbor, candidate)
          }
        }
      }
    }
    return path.concat(smallest).reverse()
  }
}

var graph = new WeightedGraph()
graph.addVertex('A')
graph.addVertex('B')
graph.addVertex('C')
graph.addVertex('D')
graph.addVertex('E')
graph.addVertex('F')

graph.addEdge('A', 'B', 4)
graph.addEdge('A', 'C', 2)
graph.addEdge('B', 'E', 3)
graph.addEdge('C', 'D', 2)
graph.addEdge('C', 'F', 4)
graph.addEdge('D', 'E', 3)
graph.addEdge('D', 'F', 1)
graph.addEdge('E', 'F', 1)

graph.Dijkstra('A', 'E')
```
