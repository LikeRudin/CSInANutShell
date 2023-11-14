# Contents

```
1. 읽기 연산을 향상시키는 방법
    - 다양한 방법
    - 핵심

2. 인덱스의 구현
    - 개요
    - 기본 타입

3. 읽기 작업 성능에대한 trade-off

```

---

# Details

# 1. 읽기 연산을 향상시키는 방법

## 1.1 다양한 방법

### 테이블 나누기: Partitioning

특정 분류에 따라 데이터를 분산시켜서 저장하는것

![](https://www.prisma.io/blog/blog/posts/indexes-and-prisma/table-partitioning.png)

- 도서관비유: 책을 보관 머릿글자의 자음별로 나눠진 선반에 보관

### 벙행 프로세스: query parallelization

쿼리를 수행하는 프로세스를 늘리는것

- 프로세스별로 서로 다른 탐색구역을 정해줌

![](https://www.prisma.io/blog/blog/posts/indexes-and-prisma/query-parallelization.png)

- 도서관비유: 여러명이 구역을 나눠 책을 탐색

### 인덱스 : Index

메타데이터를 이용해 데이터를 분류하여 저장하는것

![](https://www.prisma.io/blog/blog/posts/indexes-and-prisma/index.png)

- 도서관비유: 책을 카테고리별로 나눠진 선반에 보관

## 1.2 핵심

읽기 연산을 향상시키는 방법은 바로 개별쿼리의 탐색 스코프를 줄이는것

# 2. 인덱스

## 2.1 개요

사용자의 데이터와 별개로 탐색을 위해 DB에 저장되는 데이터
인덱스가 존재한다면, 읽기 작업 수행시 전체 데이터에 접근하지 않고,
인덱스데이터에 먼저 접근하여 대상을 검색함

- 읽기연산 향상을 위해 자주 사용됨

- 인덱스가 구현되는 자료구조의 모양은 다양하나, 모두 key:value 쌍을 저장함

## 2.2 기본 인덱스 타입

데이터 검색을 위해 사용되는것들은 전부 Index라고 할 수있음

1. default Index

일반적이고 고유하지 않은 인덱스.
자료구조의 형태와 특별한 연관을 갖고있지 않음

2. Primary Key

하나의 테이블안에서 고유한 row를 식별하기위해 사용됨

3. Unique Index

각 컬럼값의 고유값을 강조하기위해 사용됨

- 같은 데이터가 복제되는것을 예방함

4. Full Text Index

text 컬럼을 사용하여 full-text search 연산을 가능하게함.

# 3. 읽기 작업 성능에대한 trade-off

데이터의 쓰기작업을 할때 해당 데이터뿐 아니라
인덱스 데이터도 생성하기때문에 쓰기 성능이 저하됨

추가적인 저장공간과 메모리가 필요

---

# Question

### Q1

인덱스로 이루고자 하는 목표는 무엇이며, trade-off는 무엇인가요?

---

# Answer

### A1

더 빠른 읽기작업의 수행을 위해서입니다.
인덱스의 구현은 필수가아닙니다.

일반 자료 말고 인덱스를 구성하는 메타데이터를 생성해야하기때문에
쓰기작업의 성능이 저하됩니다.

---

# References

https://www.prisma.io/blog/improving-query-performance-using-indexes-1-zuLNZwBkuL#storing-books-in-smaller-shelves-partitioning
