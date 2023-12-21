# Contents

```
1. Graphs
    - Graph 개요
    - Tree가 아닌 Graph
    - 그래프 구현방식

2. 탐색 알고리즘
    - DFS
    - BFS
    - 시간복잡도
    - 구현

```

---

# Details

# 1. Graphs

## 1.1 Graph 개요

```
node와 그 사이간 edge(간선)들로 구성된 집합

G(V,E)
A collection of nodes with edges between them
```

![](https://www.tutorialspoint.com/discrete_mathematics/images/nonplanar_graph.jpg)

CS분야 전반에서 유용하게 사용되는
비선형 자료구조

비선형 자료구조

- 일반적으로 비선형이라고 간주하는게 옳으나, 선형일수도있음

## 1.2 Tree가 아닌 Graph

모든 Tree는 Graph이지만, 모든 Graph가 Tree인건 아님.

1. 간선의 방향성

   ![](https://www.researchgate.net/publication/50591619/figure/fig3/AS:667872535773189@1536244629217/a-An-example-of-undirected-graph-and-b-an-example-of-directed-graph.png)

   - Tree는 부모 노드에서 자식 노드로 향하는 방향이 존재
   - Graph는 방향성이 있을수도있고, 없을 수도있음
     - directional Graph, un-directional Graph

2. Cycle의 형성여부

   ![](https://i.stack.imgur.com/5lZ2x.png)

   - Tree에서는 Cycle이 형성되지 않음

     - 부모의 자식이 다시 부모인 막장상황은 없음

   - Graph에서는 cycle이 형성될 수있음
     - node들이 순환적으로 연결되기도 함

## 1.3 구현 방식

### 1.3.1 Adjacency Matrix (인접매트릭스)

n개의 node를 가진 Graph를
n차 정사각 행렬(Square Matrix)로 표현하는것

- 행렬의 각 성분은 두 노드 사이의 edge의 존재 여부를 표시

- y행 j열의 값은 노드 y에서 노드 j로 가는 간선이 있는지를 의미
  - 값이 1이면 해당 간선이 존재
  - 값이 0이면 간선이 존재하지 않음

### 1.3.2 Adjacency List (인접 리스트)

각 Node를 자신의 성분값과
다른 Node와의 연결 여부를 나타내는 List(또는 배열)로 표현하는것

```js
class Node {
  constructor(v) {
    this.val;
    this.edges = [];
  }
}

class Graph {
  constructor(){
    this.graph:Node[] = [];
    }
}
```

### Directional Graph

유향그래프

<div style="display:flex;">
        <div style="margin:10px">
            <img src="https://media.geeksforgeeks.org/wp-content/uploads/20230727130528/Directed_to_Adjacency_matrix.png" alt="Directed to Adjacency Matrix">
        </div>
        <div style="margin:10px">
            <img src="https://media.geeksforgeeks.org/wp-content/uploads/20230727155209/Graph-Representation-of-Directed-graph-to-Adjacency-List.png" alt="Graph Representation of Directed Graph to Adjacency List">
        </div>
    </div>

### Undirectional Graph

무향그래프

<div style="display:flex;">
        <div style="margin:10px">
            <img src="https://media.geeksforgeeks.org/wp-content/uploads/20230727130331/Undirected_to_Adjacency_matrix.png">
        </div>
        <div style="margin:10px">
            <img src="https://media.geeksforgeeks.org/wp-content/uploads/20230727154843/Graph-Representation-of-Undirected-graph-to-Adjacency-List.png" alt="Graph Representation of Directed Graph to Adjacency List">
        </div>
    </div>

# 2. 탐색 알고리즘

탐색알고리즘이 필요함

- 비선형 자료구조이니까, 선형 탐색이 불가능

## 2.1 Depth-First Search

깊이우선 탐색

그래프의 Branch를 하나씩 정복하는 탐색방식
다음 Branch로 넘어가기전에 해당 Brach를 전부 탐색

- 형제 Node를 탐사하기 전, 자식을 모두 탐사하는 방식

![](https://camo.githubusercontent.com/307023a33368ed02198844a9b3d9b8b7b470f67bbcc0e88574da939b76775c89/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f372f37662f44657074682d46697273742d5365617263682e676966)

- 주로 모든 Node를 전부 탐색해야할때 쓰인다.

## 2.2 Bredth-First Search

너비 우선탐색

자식Node로 이동하기전에, 형제를 전부 탐사하는 방식
![](https://camo.githubusercontent.com/73761db9068bf4c9de4a23209da587a29e8cc672558534d4ff40ac0480854047/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f352f35642f427265616474682d46697273742d5365617263682d416c676f726974686d2e676966)

## 2.3. 시간복잡도

인접 행렬 : O(V^2)

- 이중배열을 순회하는것과 같다.

인접 리스트 : O(V+E)

- 모든 정점을 최소 한번씩 방문함: V
- 모든 간선은 한번씩만 검토됨: E

## 2.4 구현

```ts
// 타입
interface ITraversalGraph {
  vertexNumber: number;
  edges: Map<number, number[]>;
  getVertexEdges: (vertex: number) => number[] | undefined;
  addEdge: (vertex: number, addedVertex: number) => void;

  dfsVisitVertex: (vertex: number, visited: Map<number, boolean>) => void;
  dfsWithStack: (vertex: number) => number[];
  dfsWithRecursion: (vertex: number) => number[];

  bfsVisitVertex: (edges: number[], visited: Map<number, boolean>) => void;
  bfsWithRecursion: (vertex: number) => number[];
  bfsWithQueue: (vertex: number) => number[];
}

// 테스트 코드

import { TraversalGraph } from "../graph-traversal";

describe("Graph", () => {
  test("should initialize with a given number of verticies", () => {
    const vertexNumber = 22;
    const graph = new TraversalGraph(vertexNumber);
    expect(graph.vertexNumber).toBe(vertexNumber);
    expect(graph.edges.size).toBe(vertexNumber);

    graph.edges.forEach((edgeArray) => expect(edgeArray).toEqual([]));
  });
});

describe("addEdge", () => {
  test("add Edges exactly", () => {
    const graph = new TraversalGraph(5);
    graph.addEdge(2, 3);
    graph.addEdge(1, 4);
    expect(graph.getVertexEdges(2)).toEqual([3]);
    expect(graph.getVertexEdges(1)).toEqual([4]);
  });
});

describe("getVertexEdges", () => {
  test("ged Edges of Node", () => {
    const graph = new TraversalGraph(5);
    expect(graph.getVertexEdges(1)).toEqual([]);
  });
});

describe("dfsWithStack", () => {
  test("run dfsWithStack", () => {
    const vertexNumber = 22;
    const graph = new TraversalGraph(vertexNumber);

    graph.addEdge(1, 2);
    graph.addEdge(2, 4);
    graph.addEdge(2, 5);
    graph.addEdge(2, 7);

    graph.addEdge(9, 20);

    expect(graph.dfsWithStack(25).length).toBe(0);
    expect(graph.dfsWithStack(21).length).toBe(1);

    expect(graph.dfsWithStack(2).length).toBe(4);
    expect(graph.dfsWithStack(9).length).toBe(2);
  });

  describe("dfsWithRecursion", () => {
    test("run bfs", () => {
      const vertexNumber = 22;
      const graph = new TraversalGraph(vertexNumber);

      graph.addEdge(1, 2);
      graph.addEdge(2, 4);
      graph.addEdge(2, 5);
      graph.addEdge(2, 7);

      graph.addEdge(9, 20);

      expect(graph.dfsWithRecursion(25).length).toBe(0);
      expect(graph.dfsWithRecursion(21).length).toBe(1);

      expect(graph.dfsWithRecursion(2).length).toBe(4);
      expect(graph.dfsWithRecursion(9).length).toBe(2);
    });
  });
});

// 실제 코드

export class TraversalGraph implements ITraversalGraph {
  vertexNumber: number;
  edges: Map<number, number[]>;

  constructor(vertexNumber: number) {
    this.vertexNumber = vertexNumber;
    this.edges = new Map(
      Array.from({ length: vertexNumber }, (_, index) => [index, []])
    );
  }
  getVertexEdges(v: number) {
    return this.edges.get(v);
  }

  addEdge(vertex: number, addedVertex: number) {
    const existingEdges = this.edges.get(vertex);
    this.edges.set(
      vertex,
      existingEdges ? [...existingEdges, addedVertex] : [addedVertex]
    );
  }

  dfsVisitVertex(vertex: number, visited: Map<number, boolean>) {
    visited.set(vertex, true);
    const children = this.getVertexEdges(vertex) as number[];

    for (const i of children) {
      if (!visited.get(i)) {
        this.dfsVisitVertex(i, visited);
      }
    }
  }

  dfsWithRecursion(vertex: number) {
    if (vertex >= this.vertexNumber) {
      return [];
    }
    const visited = new Map<number, boolean>();
    this.dfsVisitVertex(vertex, visited);

    const result = [...visited.keys()];

    return result;
  }

  dfsWithStack(vertex: number) {
    if (vertex >= this.vertexNumber || this.getVertexEdges(vertex)) {
      return [];
    }
    const stack: number[] = [];
    stack.push(vertex);
    const visited = new Map<number, boolean>();

    while (stack.length) {
      const target = stack.pop() as number;
      visited.set(target, true);
      const children = this.getVertexEdges(target)!;
      for (const child of children) {
        if (!visited.get(child)) {
          stack.push(child);
        }
      }
    }
    const result = [...visited.keys()];
    return result;
  }

  bfsWithQueue(vertex: number) {
    if (vertex >= this.vertexNumber) {
      return [];
    }

    const visited = new Map<number, boolean>();
    const queue: number[] = [vertex];

    while (queue.length) {
      const currentVertex = queue.shift()!;
      if (!visited.has(currentVertex)) {
        visited.set(currentVertex, true);
        const children = this.getVertexEdges(currentVertex)!;
        for (const child of children) {
          if (!visited.get(child)) {
            queue.push(child);
          }
        }
      }
    }

    return [...visited.keys()];
  }

  bfsVisitVertex(edges: number[], visited: Map<number, boolean>): void {
    if (edges.length === 0) return;

    let nextLevelNodes: number[] = [];

    for (let node of edges) {
      if (!visited.get(node)) {
        visited.set(node, true);
        let children: number[] | undefined = this.getVertexEdges(node);
        if (children) {
          nextLevelNodes.push(...children.filter((n) => !visited.has(n)));
        }
      }
    }

    this.bfsVisitVertex(nextLevelNodes, visited);
  }

  bfsWithRecursion(vertex: number) {
    let visited = new Map();
    let levelNodes = [vertex];

    this.bfsVisitVertex(levelNodes, visited);
    return [...visited.keys()];
  }
}
```

---

# Question

1. DFS/BFS의 방문 순서에대해 설명하세요.

---

# Answer

1. DFS는 현재 방문노드의 인접노드 먼저, BFS는 이전 방문노드의 인접노드를 먼저 탐색하는 방법입니다.
   Tree의경우에는 자식노드를 먼저 방문하는것이 DFS이고, 형제노드를 먼저방문하는것이 BFS입니다.

---

# References

https://ashley-gaskins.medium.com/distinguishing-bfs-and-dfs-bb413fa1b0e7

https://www.geeksforgeeks.org/graph-and-its-representations/
