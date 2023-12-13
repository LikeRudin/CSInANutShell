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
트리자료구조:

정의: edge에 의해 연결되어 계층 구조를 형성하는 Node들의 집합

특징: 비선형, 계층적
용도: 특정 data를 저장하는데에 자주 쓰임
- 탐색 및 검색 작업을 빠르게 수행할 수있음


A tree data structure is a hierarchical, non-linear structure

It is a collection of nodes
that are connected by edges and has a hierarchical relationship between the nodes.

that is used to represent and organize data
in a way that is easy to navigate and search.


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

### Tree로 만들어진것들

| 이름                         | 용도                      |
| ---------------------------- | ------------------------- |
| HTML DOM                     | 브라우저 컨텐츠           |
| Network Routing              | 네트워크 라우팅 경로 표현 |
| Abstract Syntax Tree         | 프로그래밍 언어 해석      |
| Artificial Intelligence      | ???                       |
| Folders in Operating Systems | 폴더 저장구조             |
| Computer file system         | 파일 저장 구조            |

### Abstract Syntax Tree

```c
while b ≠ 0
    if a > b
        a := a − b
    else
        b := b − a
    return a
```

![](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c7/Abstract_syntax_tree_for_Euclidean_algorithm.svg/800px-Abstract_syntax_tree_for_Euclidean_algorithm.svg.png)

# 2. 트리의 순회 (Tree Traversal)

# 3. 이진탐색트리 (Binary Search Tree)

## 3.1 정의

```
Binary Search Tree

이진 탐색 트리

자료를 번호로 빠르게 검색하기위한 트리.

```

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20221215114732/bst-21.png)

```js
class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }

  // 새로운 노드 삽입
  createNode(value) {
    const newNode = new Node(value);

    if (!this.root) {
      this.root = newNode;
      cosole.log("노드 삽입");
      return;
    }

    let comparison = this.root;

    while (true) {
      if (comparison.value > newNode.value) {
        if (comparision.left) {
          console.log("현재 Node보다 작다. 왼쪽 노드로 이동");
          comparison = comparison.left;
          continue;
        }
        comparison.left = newNode;
        console.log("노드 삽입");
        return true;

        comparison = comparison.left;
      } else if (comparison.value < newNode.value) {
        if (comparison.right) {
          console, log("오른쪽 노드로 이동");
          comparison = comparison.right;
          continue;
        }
        comparison.right = newNode;
        console.log("노드 삽입");
        return true;
      } else {
        console.log("이미 존재하는 노드입니다.");
        return false;
      }
    }
  }
    // 노드 탐색
  findNode(value) {
    let comparision = this.root;
    const locationArray = [];
    while (true) {
      if (!comparision) {
        console.log("Tree 내부에 값이 없습니다.");
        return false;
      }
      if (comparision.value > value) {
        locationArray.push(0);
        console.log("현재 Node값이 더 큼. 왼쪽 Node로 이동");
        comparison = comparision.left;
      } else if (comparison.value < value) {
        locationArray.push(1);
        console.log("현재 Node값이 더 작음. 으론쪽 Node로 이동");
        comparison = comparision.right;
      } else {
        const location = locationArray.join("");
        console.log(`Node 탐색완료. ${value}의 위치: ${location}`);
        return locationArray;
      }
    }
  }

  //노드삭제
  deleteNode(value) {
    //삭제할노드 탐색
    const targetLocation = findeNode(value);
    if(!targetLocation){
        console.log("삭제실패")
        return false
    }

    // 삭제 대상과 그 부모노드 저장
    let previousNode;
    let targetNode = this.root

    targetLocation.forEach(item => {
        if (item) {
            previousNode = targetNode;
            targetNode = targetNode.left;
            return
        }
        previousNode = targetNode;
        targetNode = targetNode.right;
        return
        });

    // 삭제대상의 자식갯수 확인
    let numOfChild = 0;

    if (targetNode.right){
        numOfChild +=1;
    }
    if (targetNode.left){
        numOfChild +=1;
    }

    // 삭제
    switch(numOfChild){
        // 대상이 자식이 없는경우
        case 0:
            if(!previousNode){
                this.root = null;
                console.log("삭제 성공")
                return true;
            }
            const isRightChild = targetLocation[targetLocation.length -1]
             if (isRightChild) {
                    previousNode.right = null;
                } else {
                    previouseNode.left = null;
                }
            return true;


            // 대상이 1개의 자식을 갖는경우
        case 1:
            // 왼쪽 자식을 갖는경우
            if (targetNode.left){
                if(!previousNode){
                    this.root = targetNode.left
                    console.log("루트 Node 삭제 성공.")
                    return true
                }
                const isRightChild = targetLocation[targetLocation.length -1]
                if (isRightChild) {
                    previousNode.right = targetNode.left;
                } else {
                    previouseNode.left = targetNode.left;
                }
                console.log("Node삭제 성공.")
                return true;

            }

            // 오른쪽 자식을 갖는경우
             if(!previousNode){
                    this.root = targetNode.right
                    console.log("루트 Node 삭제 성공.")
                    return true
                }
                const isRightChild = targetLocation[targetLocation.length -1];
                if (isRightChild) {
                    previousNode.right = targetNode.right;
                } else {
                    previouseNode.left = targetNode.right;
                }
                console.log("삭제 성공");
                return true;
        //대상이 2개의 자식을 갖는경우 맨왼쪽 맨 아래 Node와 교체해준다.
        case 2:
            let replacedParent;
            let replaced = targetNode;

            while(replaced.left){
                replacedParent = replaced;
                replaced = replaced.left;
            }
            // 옮길 Node를 기존위치에서 제거
            replacedParent.left = null;


            // 삭제 Node 위치에 대신저장

            // 삭제Node가 Root인경우
            if(!previousNode){
                    this.root = replaced;
                    console.log("루트 Node 삭제 성공.")
                    return true
                }

            const isRightChild = targetLocation[targetLocation.length -1]

            // 삭제Node가 parent의 오른쪽 자식인경우
             if (isRightChild) {
                previousNode.right = replaced;

            // 삭제Node가 parent의 왼쪽 자식인경우
            } else {
                previouseNode.left = replaced;
            }
            return true;
    }
}
```

---

# Question

1.  균형 이진 탐색트리에서 검색작업의 시간복잡도는O(log(n)) 입니다. 시간 복잡도의 도출과정을 보여주세요

---

# Answer

1.  균형 이진 탐색트리에서 n번 내려갈수있다면, 즉 트리의 레벨이 n이라면,

    노드는 개수는 약 2^n 개입니다.

    2^n개의 노드속에서 n번 이하의 이동으로 값을 찾을 수 으므로,

    자료가 n개라고 하면, 이동 횟수는 약 $$\log_2 n$$ 이 됩니다.

          따라서 시간복잡도는 O(log(n))이 됩니다.

---

# References

https://www.geeksforgeeks.org/difference-between-linear-and-non-linear-data-structures/

https://www.geeksforgeeks.org/introduction-to-tree-data-structure-and-algorithm-tutorials/

https://www.geeksforgeeks.org/binary-search-tree-data-structure/
