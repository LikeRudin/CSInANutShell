### Contents

```
1. 트리 개요
    - 정의
    - 선형/비선형 자료구조
    - 트리 용어 설명

2. 트리의 순회방식
    - 전위순회
    - 중위순회
    - 후위순회
    - 레벨순회

3. 이진탐색트리
    - 정의
    - 용도
    - 구현
```

---

# Details

# 1. 트리 개요

## 1.1 트리의 정의

```
트리:

계층 구조를 갖는 자료구조
특정 data의 탐색 및 검색 작업을 빠르게 수행할 수있음
연결되어있는 Node들과 간선의 집합으로 구성되며, cycle을 형성하지 않음

A tree data structure is a hierarchical structure
that is used to represent and organize data in a way
that is easy to navigate and search.

 It is a collection of nodes that are connected by edges and has a hierarchical relationship between the nodes.
```

## 1.2 선형/비선형 자료구조

### 선형 자료구조

데이터를 순차적으로 data를 배열한 자료구조

각 data는 이전과 다음 , 두가지 data와 연결되어있음

### 비선형 자료구조

각 data가 순차적으로 배열되어있지 않은 자료구조

![](https://media.geeksforgeeks.org/wp-content/uploads/20191010170332/Untitled-Diagram-183.png)

### 선형/비선형 비교

| 비교대상        | 선형 데이터 구조 (Linear Data Structure)                                                                             | 비선형 데이터 구조 (Non-linear Data Structure)                                                          |
| --------------- | -------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| 요소연결        | 선형 순서로 배열 <br> 각 요소는 이전 요소와 다음 요소에 연결 <br> ㅁ-ㅁ-ㅁ-ㅁ                                        | 계층적으로 연결                                                                                         |
| 레벨구분        | 단일 레벨만포함 <br> 딱히 구분 안함                                                                                  | 1개 이상의 레벨을 구분함.<br> 트리는 루트로부터의 간선 개수로 계층을 구분 <br>                          |
| 구현난이도      | 상대적으로 쉬움                                                                                                      | 상대적으로 어려움                                                                                       |
| 전체순회효율    | 단일 실행으로만 순회가능                                                                                             | 대체로 단일 실행만으로는 순회가 가능                                                                    |
| 데이터 변경효율 | 데이터를 고정, 연속적으로 저장하는 경우가 많음 <br> 변경이 많은 연산을 요구                                          | 데이터를 연속적으로 저장하는경우가 거의없음 <br> 임의저장 방식으로 저장하여 변경 연산 효율이 좋음       |
| 사용되는 분야   | 주로 응용 소프트웨어 개발에 이용                                                                                     | 여러가지 분야에 이용                                                                                    |
| 구체적 용도     | 단순한 데이터 저장 및 조작                                                                                           | 데이터 저장 외에도 복잡한 관계 및 데이터 계층을 나타내는 데 유용 <br> Network, File System등에서도 사용 |
| 연산 성능       | 간단한 연산 성능은 보통 좋지만, 중간 요소를 검색하거나 제거하는 연산은 느림 <br> 연산 대상의 위치에 따라 효율이 바뀜 | 특정 연산(DB의 경우 읽기)등에 최적화                                                                    |
| 예시            | Array, Stack, Queue, Linked-List                                                                                     | Tree, Graph                                                                                             |

## 1.3 트리 용어 설명

![](https://media.geeksforgeeks.org/wp-content/uploads/20221124153129/Treedatastructure.png)

| Terminology                          | 설명                                                                                                                                                                     |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 부모노드(Parent Node)                | 이 노드를 향하는 간선을 가진 노드 <br> {B}는 {D, E}의 부모노드                                                                                                           |
| 자식노드(Child Node)                 | 한 노드로부터 나가는 간선이 향하는 노드 <br> {D, E}는 {B}의 자식노드                                                                                                     |
| 루트노드(Root Node)                  | 트리의 최상위 노드 (부모노드가 없음) <br> {A}는 트리의 루트 노드 <br> +@ 비어 있지 않은 트리는 반드시 하나의 루트 노드로부터 다른 모든 노드로 가는 단 하나의 경로를 가짐 |
| 리프노드(Leaf Node or External Node) | 자식노드가 없는 노드 <br> {K, L, M, N, O, P, G}는 트리의 리프노드                                                                                                        |
| 선조노드(Ancestor of a Node)         | 한 노드에대해, 루트노드부터 그 노드까지 이어지는 경로상의 모든 노드 <br> {A, B}는 노드 {E}의 선조노드                                                                    |
| 후손노드 (Descendant)                | 한 노드에서 간선으로 이동할 수있는 경로에 위치한 모든 노드들 <br> {E, I}는 노드 {B}의 후손                                                                               |
| 형제노드 (Sibling)                   | 같은 부모 노드를 갖는 노드들 <br> {D, E}는 형제노드                                                                                                                      |
| 노드 레벨 (Level of a node)          | 루트 노드에서 그 노드까지의 경로에 있는 간선의 수. <br> 루트 노드의 레벨은 0                                                                                             |
| 내부노드 (Internal node)             | 자식노드를 갖는 노드                                                                                                                                                     |
| 이웃한 노드 (Neighbour of a Node)    | 방향과 관계없이 간선으로 연결된 노드                                                                                                                                     |
| 서브트리 (Subtree)                   | 트리에서 특정 노드를 루트노드로 고르고, 기존의 간선에 포함되게 그 자손을 골라서 분리한 부분트리                                                                          |

# 2. 트리의 순회 (Tree Traversal)

# 3. 이진탐색트리 (Binary Search Tree)

---

# Question

---

# Answer

---

# References

https://www.geeksforgeeks.org/difference-between-linear-and-non-linear-data-structures/

https://www.geeksforgeeks.org/introduction-to-tree-data-structure-and-algorithm-tutorials/
