# Contents

```
    1. 알고리즘 개요
        - 알고리즘 정의
        - 알고리즘 평가

    2. 알고리즘 복잡도
        - 개요

        - Big O
        - Big Θ
        - Big Ω

```

---

# Details

# 1. 알고리즘 개요

## 1.1 알고리즘(Algorithms)

### 알고리즘의 사전적 의미 

```
특정 문제의 해결을 목적으로 하는 행동 절차, 순서를 갖는 규칙

A set of rules to be followed in calculations or other problem-solving operations

A procedure for solving a mathematical problem in a finite number of steps that frequently involves recursive operations
```

### Algorithm in CS

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20191016135223/What-is-Algorithm_-1024x631.jpg)

주어진 입력으로부터 원하는 출력 도출할 수있는 규칙의 집합

## 1.2 알고리즘 평가

문제에 따라서 최적의 성능을 낼 수있는 알고리즘은 다르다.

그래서 컴퓨터 과학자들은 각 문제 해결을 위한 최적의 알고리즘을 찾기 위해 노력한다.

이때, 알고리즘은 반드시 특정 `문제`와 함께 평가된다.


### 평가항목

1. 정확성 (Accuracy)

   - 문제를 정확하게 해결할 수 있는지 여부

2. 복잡도 (complexity)

   - 알고리즘이 문제를 해결하는데 걸리는 자원의 양

3. 간단성 (simplicity)

   - 쉽게 이해 할 수 있는지 평가할 수 있는 정도

4. 최적성 (Optimality)

   - 알고리즘이 해당 문제를 해결하는데 적절한가

- 평가항목의 중요도

일단은 문제를 해결 할 수있어야 하므로, 
가장 기본적으로 충족되어야하는것은 `정확성` 이다.

다음으로 중요한 평가요소는 `복잡도` 이다. 
시간, 메모리 등 연산에 사용할 수있는 자원은 한정적이기 때문이다.
특정 장비, 하드웨어 및 소프트웨어 환경이 해당 알고리즘을 수행하기위해 적절한지 알 수 있어야 하므로, 굉장히 중요하다

 # 복잡도 표기법
 
일반적으로 `점근 표기법`(asymptotic notation)으로
복잡도를 표기한다.

- Big O, Big Θ, Big Ω


## 2.1 Big O

![](https://media.geeksforgeeks.org/wp-content/uploads/AlgoAnalysis-2.png)

```
정의:
g와 f는 정의역과 공역을 자연수 집합으로 갖는 함수이다.


만약에 어떤 양수 c와 자연수 n0이 존재해서,
n0 ≤ n 을 만족하는 모든 n에대해
 f(n) ≤ cg(n) 이면

f를 O(g), 즉 g의 big O라고 부른다.



Let g and f be functions from the set of natural numbers to itself. The function f is said to be O(g) (read big-oh of g), if there is a constant c > 0 and a natural number n0 such that f(n) ≤ cg(n) for all n ≥ n0 .
```

## 2.2 Big Omega

![](https://media.geeksforgeeks.org/wp-content/uploads/AlgoAnalysis-3.png)

```
두 함수 f와 g에대해, 어떤 양수 c와 자연수 n0이 존재해서,
n0이상인 모든 자연수 n에대해 다음 부등식을 만족할때
f를 g의 big Omega라고 부른다.

 0 ≤ cg(n) ≤ f(n)

Ω(g(n)) = {
    f(n): there exist positive constants c and n0
     such that 0 ≤ cg(n) ≤ f(n) for all n ≥ n0
}
```

## 2.3 Big Theta

![](https://media.geeksforgeeks.org/wp-content/uploads/20220822015828/thetanotation.png)

```
두 함수 f와 g에대해,

어떤 양수 c1과 c2, 그리고 자연수 n0이 존재하여
n0이상인 모든 자연수 n에 대하여 다음 식을 만족하면

f를 g의 big Thetha라고 부른다.

0 ≤ c1 * g(n) ≤ f(n) ≤ c2 * g(n)

Θ (g(n)) =
{
   f(n): there exist positive constants c1, c2
   and n0 such that 0 ≤ c1 * g(n) ≤ f(n) ≤ c2 * g(n) for all n ≥ n0
}
```

### 이상한점 없으셨나요?

위의 Big-O, Big-Theta, Big-Omega는 사실 집합입니다.
우리가 일반적으로 사용하는 용어로 변모하려면 가공을 접해야합니다.

Big-O 집합에서 변화율이 가장 가파른 종류의 함수를 가져오고
Big-Omega 집합에서 변화율이 완만한 종류의 함수를 가져와야
컴퓨터 과학에서의 Big-O, Big-Omega notation에 쓰이는 함수가 됩니다.

# Question

### 1. Big-Omega, Big-theta, Big-O 중에 왜 Big-O를 가장 주요한 알고리즘의 시간복잡도 평가 지표로 사용하나요?

### 2. 일반적인 함수식에서 Big-O 표기로 바꾸는법을 설명해보세요.

---

# Answer

### 1. Big-Omega, Big-theta, Big-O 중에 왜 Big-O를 가장 주요한 알고리즘의 시간복잡도 평가 지표로 사용하나요?

그 이유는 Big-O가 최악의경우에대한 시간복잡도를 나타내기 때문입니다.
최선의 경우나 평균적인 경우보다는, 최악의경우를 상정하고 최대성능을 발휘할 수있게 설계해야 안정적인 성능을 발휘할 수있습니다.

### 2. 일반적인 함수식에서 Big-O 표기로 바꾸는법을 설명해보세요.

1. 최고차항만 남깁니다.

   - 승수가 제일 높은 한개의 항만 남깁니다

2. 계수를 전부 제거합니다.

   - 숫자계수를 전부 제거합니다.

---

# References

https://www.geeksforgeeks.org/types-of-asymptotic-notations-in-complexity-analysis-of-algorithms/
https://www.geeksforgeeks.org/fundamentals-of-algorithms/
