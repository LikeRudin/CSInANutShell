# Contents

```
    1. 개요
        - 정의
        - 사용기법: memoization
    2. 예시
        - 최단경로찾기
        - 피보나치
```

---

# Details

# 1. 개요

## 1.1 정의

### Dynamic Programming

동적 계획법

큰 규모의 문제를 작은 부분으로 나누고
각 부분에대한 최적해를 찾아 해결하는것

- 최적 부분구조
  - 이런 방식으로 문제를 나눌수 있다면,
    문제가 최적의 부분구조를 가진다고 표현

```

if a problem can be solved optimally by breaking it into sub-problems and then recursively finding the optimal solutions to the sub-problems, then it is said to have optimal substructure.
```

## 1.2 사용 기법

### memoization: 기억시키기.

큰 규모의 문제를 부분으로 나눠서 각 부분별 최적해를 구한후,
이를 이용해 문제를 해결

- 각 부분별 최적해를 `이용` 해야함

  - 최적해를 `기억`(memoization)시켜 놓아야함
  - 이미 해결한 부분 문제를 다시 해결하지 않음 => 성능 향상

# 2. 예시

## 2.1 최단경로 찾기

A에서 D로가는 최단경로를 찾기
![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3b/Shortest_path_with_direct_weights.svg/1200px-Shortest_path_with_direct_weights.svg.png)

B나 E를 통해서 D 에 갈 수있음
각 경로별로 값을 새로 계산하지않고 각 경로값을 기억하며 진행

예시: 한글 자음변수에 경로값을 저장하며 계산

- ㄱ: A => B: 4
- ㄴ: B => D: 10
- ㄷ: A => B => D: 14 (ㄱ + ㄴ)👌

- ㄹ: A => C: 2 (A=>C:2 < ㄱ + B=>C: 5)
- ㅁ: C => E: 3
- ㅂ: A => C => E: 5 (ㄹ + ㅁ)
- ㅅ: E => D: 4
- ㅇ: A => C => E => D: 9 (ㅂ + ㅅ)👌

- ㅈ: A => D: 9 (ㅇ < ㄷ)

최소경로값: 9

- 동적 계획법이 적용된 포인트

  - 중간중간 새로운 경로를계산할때마다, 이전의 저장값을 이용함

  - ㅇ에서 경로를 구할때, A => C + A => E를 다시 계산하지 않고
    이전에 ㅂ에 저장해놓은 값을 이용함

## 2.2 피보나치 수열의 합

Dynamic Programming이 적용되는 가장 대표적인 예시

피보나치트리

## ![](https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/Fibonacci_Tree_5.svg/558px-Fibonacci_Tree_5.svg.png?20070721115329)

- 말단노드(terminal node) 값: 1
- 말단노드의 부모 노드 값: 2
- 그위 노드들의 값: 두자식의 값의 합

수열 트리에서 맨 왼쪽 말단 노드부터 루트 노드까지 경로상에의 모든 왼쪽 자식 노드들의 값을 구하기
=> 각 노드에 값을 저장해두고 이를 재사용함

# Question

### 1. Dynamic programming 의 핵심 원리에대해 설명해주세요.

---

# Answer

### 1.Dynamic programming 핵심 원리에대해 설명해주세요.

Dynamic Programming은 큰 문제를 작은 부분 문제로 나눈 뒤, 각 부분 문제에 대한 해답을 찾고 이를 활용하여 원래 문제의 답을 도출하는 성능 최적화 기법입니다.

각 부분 문제의 해답이 다른 큰 부분 문제의 해답을 구하는 데 사용될 수 있어야 합니다.
그러므로 계산의 중복을 방지하기 위해 최적의 해답을 저장하고 기억하는 메모이제이션(Memoization)이 Dynamic programming의 핵심이라고 할 수있습니다.

---

# References
