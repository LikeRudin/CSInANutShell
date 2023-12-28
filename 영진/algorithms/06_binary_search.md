# Contents

```
이진탐색

    1. 개요
        - 정의
        - 구현

    2. 이진 탐색트리


```

---

# Details

# 1. 개요

## 1.1 Binary Search 정의

이진탐색

탐색알고리즘 (Searching algorithm)의 일종이다.

- 용도: 미리 정렬된 Array 내부에서 특정 값을 빠르게 검색
- 핵심 아이디어: 검색 구간을 계속해서 반으로 나눠 시간복잡도를 줄임

```

Binary Search is defined as a searching algorithm used in a sorted array by repeatedly dividing the search interval in half. The idea of binary search is to use the information that the array is sorted and reduce the time complexity to O(log N).
```

## 1.2 구현

![](https://media.geeksforgeeks.org/wp-content/uploads/20220309171621/BinarySearch.png)

1. 정렬된 선형 자료구조를 준비한다.
2. 탐색범위를 정한다
   - 첫번째 인덱스`first`와 마지막 인덱스`last`
3. 탐색범위의 가운데 인덱스`mid`를 구한다
4. `mid`에 저장된 값과 찾는 값`searching`을 비교한다.
   - 값이 같으면, 탐색을 종료한다
5. 탐색 범위를 갱신한다.
   - `searching`보다 `mid` 값이 작은경우 `mid + 1`을 새로운 `first`로 삼는다
   - `searching` 보다 `mid` 값이 더 큰경우 `mid - 1`을 새로운 `last`로 삼는다.
   - `first`나 `last`가 기존 자료구조의 범위를 초월할경우, 검색을 종료한다.
6. 3-4-5 과정을 재귀적으로 반복한다.

```ts
const binarySearch = (array: number[], searchingValue: number) => {
  const sortedArray = array.toBeSorted();
  let [first, last] = [0, sortedArray.length - 1];
  let mid;

  while (last > 0) {
    mid = Math.floor((last - first) / 2);
    const midValue = sortedArray[mid];
    if (searchingValue === midValue) {
      return mid;
    }
    if (searchingValue > midValue) {
      first = mid + 1;
    } else {
      last = mid - 1;
    }
  }
  return undefined;
};
```

# 2 이진 탐색 트리

이진탐색을 위한 트리

![](

## ![](https://miro.medium.com/v2/resize:fit:640/format:webp/1*OmRV7P0YluY2ToRj44jKGA.gif)

)
부모와 자식의 값을 비교하면
왼쪽 자식은 언제나 부모보다 작고, 오른쪽은 언제나 부모보다 크다

- 이진 탐색트리를 활용하면 Mid를 계산하거나, 탐색범위를 갱신할 필요가 없음

# Question

### 1. 선형 자료구조에서 이진 탐색을 구현할때, 이진탐색 알고리즘의 차이점을 말씀해주세요

---

# Answer

### 1. 선형 자료구조에서 이진 탐색을 구현할때, 핵심적인 부분을 두가지만 제시해보세요

- 미리 정렬된 자료구조, 탐색범위를 계속해서 반으로 줄임.

### 2. 선형 자료구조에서와 이진 탐색트리에서의 이진탐색 알고리즘의 차이점을 말씀해주세요

미리 특정 규칙에 의해 정렬된 자료구조를 준비해야한다는 점은 동일합니다.

하지만 이진 탐색트리는 더 간소화된 알고리즘을 구현할 수있습니다.
그 이유는 다음과 같습니다.

1. 가운데 인덱스를 계산하는 로직이 필요없습니다.
   언제나 현재 검사중인 노드의 값과 탐색중인 값을 비교하면됩니다.

2. 탐색범위를 계산해서 갱신하는 로직이 필요없습니다.

현재 노드보다 탐색중인 값이 더크면 오른쪽 자식노드로 이동합니다.
반면, 탐색중인 값이 더 작다면 왼쪽 자식노드로 이동합니다.
이 과정을 통해 탐색범위는 자동으로 반으로 줄어들며 갱신됩니다.

---

# References

https://www.geeksforgeeks.org/binary-search/

https://medium.com/swlh/how-to-solve-a-js-binary-search-tree-problem-585673fc3287
