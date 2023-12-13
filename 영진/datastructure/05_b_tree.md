# Contents

```
    1. B -Tree
        - 개요
        - 특징
        - 사용처
    2. B + Tree
        - 개요
        - 특징
        - 사용처

    3. B-Tree vs B+Tree
```

---

# Details

# 1. B-Tree

## 1.1 개요

### B-tree

Balenced Tree

효율적인 파일 저장 및 탐색을 위해 Binary Search Tree를 개선한 Tree

하나의 Node에 여러개의 Key를 저장할 수있음.

- Node 하나가 가질수있는 최대 Key의 갯수를 Branching Factor라고함

Branching Factor가 6인 Tree
![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*euhePwmpxpKHBg0aI2Qlvg.png)

## 1.2 특징

### DISK I/O작업 횟수가 작다.

하나의 Node에 많은양의 Key를 집어넣어, Height가 낮아졌고,
Node 하나를 Page로 취급해서, 통째로 읽어오기때문에

DISK I/O 작업의 횟수를 줄여서 검색 성능을 향상시켰다.

### SSD에서 사용하기엔 불리하다.

하나의 Node를 수정할때, SSD는 해당 Node를통째로 다시쓰기때문에, 성능 손해를 보는 구조이다.

### Self-balancing Tree

insert, delete 등의 작업후에도 트리의 높이가 최소화되게하도록
스스로 조정하는 알고리즘을 갖는 Tree

Tree의 검색효율은 부모-자식관계의 edge를 몇번타고내려가느냐에 달려있음
높이를 가능한 최소로 유지해야 균형있는 검색 성능을 보장할 수있음.

새로운 key를 삽입할때, Node에 Branching Factor 만큼의 key가 들어있지않다면,
해당 Node에 key를 삽입하고, Branching Facotr에 도달했을때는 다음과정을 거쳐 Key를 삽입함

1. Splitting
   해당 Node를 한개의 key를 기준으로 반으로 가름

2. Promotion
   중간에 위치한 Key를 부모 노드로 상향시킴

3. NodeCreation
   half-full 상태의 Node를 두개 만듦
   중간키 미만 / 중간키 초과

4. Rebalancing
   위의 과정이 끝난후 전체 Tree의 균형을 재조정함

## 1.3 사용처

느리지만 커다란 데이터를 저장하는곳에 많이 쓰임.
hard drives, flash memory, CD-ROM 등

- 항목 하나를 수정할때, Page를 통째로 다시써야하는 SSD에겐 불리한 자료구조

각 Node(page)는

### 시간복잡도

Search, Insert, Delete 모두 O(log(n)) 의 시간복잡도를 가짐

# 2. B+Tree

### 2.1 개요

B-Tree의 변형으로, Leaf Node를 LinkedList로 구현한 트리.

### 2.2 특징

### 2.2.1 LinkedList로 구현된 Leaf Node.

Tree의 한계를 극복하여 `순차적 검색`을 수행할 수있음

- Tree는 비선형 자료구조
  - 특정 자료 하나를 빠르게 찾을수있음
  - 1번의 실행으로 전체 자료를 순회하는것은 불가능
  - 순차적 검색이 불가능

### 2.2.2 Data Pointer는 Leaf Node에 위치함.

Data를 가리키는 DataPointer는 Leaf Node에만 존재하며,
다른 Node(page)는 하위 Node(page)의 참조만 포함하고 있음

<div style="display: grid; grid-template-columns: 1fr 1fr;">
    <div>  <p>Internal Node</p>
        <img src="https://media.geeksforgeeks.org/wp-content/uploads/20230623132532/ezgifcom-gif-maker-(12)-768.webp" alt="B+ Tree Diagram" style="height: 80%; width: 90%;">
       </div>
    <div>
     <p>Leaf Node</p>
   <img src="https://media.geeksforgeeks.org/wp-content/uploads/20230623132641/ezgifcom-gif-maker-(13)-768.webp" alt="B Tree Diagram">
    </div>
</div>

## 2.3 사용처

B-Tree가 사용되는 모든곳

특히 Multilevel Indexing이나 빠른 작업이 필요한곳에 사용

# 3. 비교

<div style="display: flex;">
    <img src="https://media.geeksforgeeks.org/wp-content/uploads/20191219160544/Untitled-Diagram111.png" alt="B+ Tree Diagram" style="margin-right: 20px; width:50%;">
    <img src="https://media.geeksforgeeks.org/wp-content/uploads/Btree.jpg" alt="B Tree Diagram" style="width:50%;">
</div>

| 기준         | B - 트리                       | B+ 트리                                              |
| ------------ | ------------------------------ | ---------------------------------------------------- |
| data pointer | Root를 제외한 모든 Node가 보유 | Leaf Node만 보유                                     |
| 검색 성능    | 상대적으로 느림                | 더 빠르고 정확함 <br> -모든 Node가 Leaf에만 있기때문 |
| 중복 키      | X                              | O <br> Leaf에는 모든 key가 존재                      |
| 삽입         | 상대적으로 오래걸림            | 쉽고 멱등성이 보장됨                                 |
| 삭제         | 트리가 많은 변형을 겪음        | 쉬움                                                 |
| LeafNode형태 | 자식이 없는 일반적인 Node      | LinkedList 형태임                                    |
| 순차적접근   | X                              | O                                                    |
| 높이         | 더 높음                        | 더 낮음                                              |
| 적용         | 데이터베이스, 검색 엔진        | 멀티 레벨 인덱싱, 데이터베이스 인덱싱                |
| 노드 수      | 중간 레벨 'l'의 노드 수는 2l임 | 각 중간 노드는 n/2에서 n개의 자식을 가질 수 있음     |

---

# Question

1. 비선형 자료구조와 선형 자료구조와 데이터검색방식의 차이를 설명해주세요.
   그리고 또, B + Tree는 어떤방식을 사용하는지 간단히 설명해주세요.

2. B-Tree와 B+ Tree는 data pointer의 저장 위치가 어떻게 다른가요? 그것은 검색연산 수행에서 어떤 차이를 만드나요?

# Answer

1. 데이터 검색에서 비선형 자료구조는 선형 자료구조와 어떤 차이를 갖나요?
   그리고 그와 관련해서 B + Tree는 어떤 특징을 갖나요?

비선형 자료구조는 특정 하나의 데이터를 빠르게 검색하기위해 사용합니다.
하지만 한번의 실행으로 전체 자료를 순회하는등의 순차적 검색이 불가능합니다.

선형 자료구조에서는 순차적 검색, 단일연산 순회 등이 모두 가능합니다.

B+ Tree는 전체적으로 비선형 자료구조 이지만
Leaf Node는 모든 DataPointer를 포함하고있는 LinkedList입니다.
그래서 선형 자료구조, 비선형 자료구조의 검색방식을 전부 사용할 수있습니다.

2. B-Tree와 B+ Tree의 data pointer 저장 위치는 어떻게 다른가요?
   그것이 검색연산 수행에서 어떤 차이를 만드나요?

   B+Tree는 모든 data pointer를 Leaf Node에만 저장해서, 전체적으로 균일한 검색성능을 보장합니다.

   B-Tree는 각 Node에 dapa poiter와 다른 Node(page)를 참조주소(혹은 ponter)를 전부갖고있어서
   검색 대상이 들어있는 Node가 Root에 가까울수록 더 빠른 검색이 수행됩니다.

# Reference

---

https://www.geeksforgeeks.org/insert-operation-in-b-tree/
https://www.geeksforgeeks.org/introduction-of-b-tree-2/
