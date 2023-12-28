# Contents

```
Dijkstra Algorithm

    1. 개요
        - 정의
        - 사용되는 곳
    2. 구현

```

---

# Details

# 1. 개요

## 1.1 정의

다익스트라 알고리즘

가중치가 음수인 간선(negative-edge)이 없는 graph에서
두 지점간 최단거리를 구하는 알고리즘

```
절차

1. 출발노드를 정함

2. 기록용 자료구조를 만듦
    - 방문 노드 보관용
    - 출발 노드로부터 각 노드까지의 최단거리를 보관용

3. 출발노드와 그 인접노드 사이에있는 경로의 가중치로 각 경로별 최단거리 초기화

4. 출발 노드로부터 가장 가중치가 작은 경로를 가진 인접 노드를 방문

5. 방문한 노드에서 연결된 경로의 가중치를 확인하여, 최단경로 정보를 갱신

    - 방문노드에게는 인접노드이지만, 출발노드에게는 아닌경우
        - 출발노드 => 방문노드, 방문노드 => 해당노드 경로의 가중치합을 저장

    - 출발노드와 방문노드 모두에게 인접한 노드
        - 출발노드와의 경로 가중치와, 출발노드 => 방문노드 =>해당경로 가중치 합을 비교하여 더 적은 값을 저장

7. 목적지에 도착하거나 가능한 모든 노드를 방문할 때까지 위 과정을 반복
```

![](<![](https://media.geeksforgeeks.org/wp-content/uploads/20230224110301/dijkstras1.png)>)

```
0  -> 0
1  -> 2
2  -> 6
3 -> 7
4 -> 17
5  -> 22
6  -> 19
```

- 탐욕법(Greedy)의 전형적인 예시

- Dynamic Programming의 memoization 원리를 사용
  - 최단경로값을 저장하고, 다른 경로 계산에 사용
  - 하지만 DP는 일반적으로 부분해를 전부 저장해놓으나, Dijkstra는 갱신하면서 진행

```
algorithms for solving many single-source shortest path problems having non-negative edge weight in the graphs i.e., it is to find the shortest distance between two vertices on a graph.

The algorithm maintains a set of visited vertices and a set of unvisited vertices. It starts at the source vertex and iteratively selects the unvisited vertex with the smallest tentative distance from the source. It then visits the neighbors of this vertex and updates their tentative distances if a shorter path is found. This process continues until the destination vertex is reached, or all reachable vertices have been visited.
```

## 1.2 사용되는곳

특정 정점에서 다른 정점들까지의 최단거리를 계산해야 하는 모든곳

- 네비게이션 GPS, 네트워크라우팅, 지하철 및 도시 도로 계획, 유투브 영상추천알고리즘..

- 각 단계별로 계산을 수행하고 저장하며 최적의 답안을 선택하는 모든곳

- AI는 각 단어별 로 가중치를 저장한 후, 단어별 연관성 수치를 계산하여 가장 적절한 답안을 생성
  - 다익스트라 알고리즘을 사용하진 않으나, 추상적으로 같은 레벨의 알고리즘을 사용

# 2. 구현

```ts
type QueueElementType<T> = {
  element: T;
  priority: number;
};

interface IPriorityQueue<T> {
  queue: QueueElementType<T>[];
  enqueue: (element: T, priority: number) => void;
  dequeue: () => null | T;
  isEmpty: () => boolean;
}

export class PriorityQueue<T> implements IPriorityQueue<T> {
  queue: QueueElementType<T>[];
  constructor() {
    this.queue = [];
  }

  isEmpty() {
    return this.queue.length === 0;
  }

  enqueue(element: T, priority: number) {
    this.queue.push({ element, priority });
    this.queue.sort((a, b) => a.priority - b.priority);
  }

  dequeue() {
    if (this.isEmpty()) {
      return null;
    }
    return this.queue.shift()!.element;
  }
}

export interface IWeightGraph {
  vertexNumber: number;
  edges: Map<number, Map<number, number>>;
  getVertexEdges: (vertex: number) => Map<number, number> | undefined;
  addEdge: (vertex: number, addedVertex: number, weight: number) => boolean;
  printAllShortestPath: (source: number) => void;
}

export class WeightGraph implements IWeightGraph {
  vertexNumber: number;
  edges: Map<number, Map<number, number>>;
  constructor(vertexNumber: number) {
    this.vertexNumber = vertexNumber;
    this.edges = new Map(
      Array.from({ length: vertexNumber }, (_, index) => [index, new Map()])
    );
  }
  getVertexEdges(vertex: number) {
    return this.edges.get(vertex);
  }
  addEdge(vertex: number, addedVertex: number, weight: number) {
    if (
      vertex >= this.vertexNumber ||
      vertex < 0 ||
      addedVertex >= this.vertexNumber ||
      addedVertex < 0
    ) {
      return false;
    }
    this.getVertexEdges(vertex)?.set(addedVertex, weight);
    this.getVertexEdges(addedVertex)?.set(vertex, weight);
    return true;
  }

  printAllShortestPath(source: number) {
    if (source < 0 || source >= this.vertexNumber) {
      return;
    }
    const priorityQueue = new PriorityQueue<number>();
    const destination = new Array<number>(this.vertexNumber).fill(Infinity);
    const visited = new Set<number>();

    priorityQueue.enqueue(source, 0);
    destination[source] = 0;

    while (!priorityQueue.isEmpty()) {
      const current = priorityQueue.dequeue()!;

      if (visited.has(current)) {
        continue;
      }
      visited.add(current);
      for (const [vertex, weight] of this.getVertexEdges(current)!.entries()) {
        const savedDistance = destination[vertex];
        const newCalculatedDistance = destination[current] + weight;

        if (!visited.has(vertex) && newCalculatedDistance < savedDistance) {
          destination[vertex] = newCalculatedDistance;
          priorityQueue.enqueue(vertex, destination[vertex]);
        }
      }
    }
    console.log(`${source}로부터 각 점까지의 최단거리:`);
    visited.forEach((vertex) => {
      console.log(`${vertex}: ${destination[vertex]}`);
    });
  }
}
```

![](https://media.geeksforgeeks.org/wp-content/uploads/20230224110301/dijkstras1.png)
![](<https://media.geeksforgeeks.org/wp-content/uploads/20230224120836/dijkstras6-(1).png>)

# Question

### 1. 다익스트라 알고리즘은 때로는 Greedy, 때로는 Dynamic Programming의 예시로 소개되곤 합니다. 다익스트라 알고리즘이 가진 두 기법의 특징을 설명하고, 엄밀하게 둘중 어떤 기법인지 정해주세요.

### 2. 다익스트라 알고리즘 구현의 진행 순서를 제시해보세요.

---

# Answer

### 1. 다익스트라 알고리즘은 때로는 Greedy, 때로는 Dynamic Programming의 예시로 소개되곤 합니다. 다익스트라 알고리즘이 가진 두 기법의 특징을 설명하고, 엄밀하게 둘중 어떤 기법인지 정해주세요.

다익스트라 알고리즘은 이전 경로까지의 최단 거리를 저장하고 이용하는 점에서 Dynamic Programming의 특징을 갖고 있습니다.
또, 현재 단계에서 도달할 수 있는 경로를 최단 거리라고 저장하고
나중에 다시 갱신하는 점에서 Greedy 특징도 가지고있습니다.

엄밀히 말하자면, 다익스트라 알고리즘은 GReedy의 일종입니다.
정확한 최단경로를 몰라도 현재가진 정보를기반으로 최단경로라고선언하기 때문입니다.

이러한 갱신과정에서 이전에 최단경로라고 여겨졌던 값은삭제하므로 ,
정보를 지속적으로 저장하고 기억하며 진행하는 Dynamic programming과는 조금 차이가 있습니다.

### 2. 다익스트라 알고리즘 구현의 진행 순서를 제시해보세요.

```
1. 방문한 노드와 아직 방문하지 않은 노드를 저장하는 두 집합을 만듭니다

2. 출발노드로부터 각 노드까지의 최단거리를 저장할 자료구조를 만듭니다

3. 출발 노드와 인접한 노드 사이에있는 경로의 가중치를 각 경로별 최단거리로 초기화합니다

4. 출발 노드로부터 가장 가중치가 작은 경로를 가진 인접 노드를 방문합니다

5. 방문한 노드에서 다른 노드로 연결된 경로의 가중치를 확인하며, 최단경로 정보를 갱신합니다
    - 방문노드에게는 인접노드이지만, 출발노드에게는 아닌경우
        - 출발노드 => 방문노드, 방문노드 => 해당노드 이렇게 두 경로의 가중치합을 저장합니다
    - 출발노드와 방문노드 모두에게 인접한 노드인경우
        - 출발노드와의 경로 가중치와, 출발노드 => 방문노드, 방문노드 =>해당노드 가중치합을 비교해서 더적은값을 출발노드로부터의 최단경로로 저장합니다

7. 목적지에 도착하거나 가능한 모든 노드를 방문할 때까지 위 과정을 반복합니다
```

---

# References

https://www.geeksforgeeks.org/introduction-to-dijkstras-shortest-path-algorithm/?ref=lbp

https://m.blog.naver.com/ndb796/221234424646

```

```
