# Contents

```
    1. Heap 개요
        - Complete Binary Tree
        - Priority Queue

    2. 구현
        - 자료구조
        - 연산
            - Insertion
            - Delete
```

---

# Details

# 1. Heap 개요

우선순위가 가장 높은 노드를 언제나 Root에 유지하는
Complete Binary Tree 기반의 자료구조

- 주로 Priority Queue의 구현에 사용

### Complete Binary Tree

Binary Tree의 일종

![](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*CMGFtehu01ZEBgzHG71sMg.png)

- 마지막 level에 포함되지 않는 모든 Node가 2개의 Child Node를 갖음
  - 최소높이를 유지하며 균일한 자료검색 성능을 제공

### Priority Queue

우선순위가 높은 data를 먼저 처리하기위한 자료구조

- FIFO방식의 Queue와는 전혀 다름

![](https://media.geeksforgeeks.org/wp-content/uploads/20230313172946/mh8.png)

# 2. 구현

## 2.1 자료구조

![](https://media.geeksforgeeks.org/wp-content/uploads/20230323095300/maxh.png)

Binary Tree는 각 레벨별로 `2^n -1` 개의 Node를 갖기때문에,
배열에 충돌 없이 저장할 수 있음.

```
현재 Node의 인덱스 계산법:

level * 2 + x축 좌표

index: Heap[i]

parent: Heap[(i-1)/2]
left Child: Heap[(2*i)+1]
right Child: Heap[(2*i)+2]
```

## 2.2 연산

![](https://algs4.cs.princeton.edu/24pq/images/heap-ops.png)

### 2.2.1 Insertion

### 2.2.2 Delete

---

# Question

1. Max-Heap 자료구조와 Insertion, Delete 연산을 배열을 이용해 구현해보세요

---

# Answer

1.

# reference

https://stevenschmatz.gitbooks.io/data-structures-and-algorithms/content/281/lecture_11.html
https://www.geeksforgeeks.org/heap-data-structure/
https://towardsdatascience.com/5-types-of-binary-tree-with-cool-illustrations-9b335c430254
