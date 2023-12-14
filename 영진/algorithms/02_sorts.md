# Contents

```
    1. 정렬 개요
        - 정렬
        - 안정/불안정


    2. 정렬 종류
        1. quick
            - 개요
            - 구현 개요
            - 구현 코드
            - 시간복잡도
        2. merge
            - 개요
            - 구현 개요
            - 구현 코드
            - 시간복잡도
        3. heap
            - 개요
            - 구현 개요
            - 구현 코드
            - 시간복잡도

```

---

# Details

# 1. 정렬 개요

## 1.1 정렬

```
많은 수의 원소들을 숫자의 대소관계나 알파벳의 사전배열과 같은
일정한 순서로 열거하는 방식, 알고리즘

 a method for reorganizing a large number of items into a specific order, such as alphabetical, highest-to-lowest value or shortest-to-longest distance.
```

## 1.2 안정/불안정 (stable/unstable)

안정(stable) 정렬

- 정렬 대상안에 동일한 key를 갖는 두 오브젝트가 있을때,
  그 오브젝트간 순서가 정렬 전후에 바뀌지않는 정렬

불안정(unstable)정렬

- 동일한 key를 갖는 두 오브젝트끼리도 순서가 바뀔 수있는 정렬

![](https://en.m.wikipedia.org/wiki/File:Sorting_stability_playing_cards.svg)

# 2 정렬 종류

## 2.1 퀵 정렬 (Quick Sort)

### 개요

(오름차순 기준)

기준점(pivot)을 선정해 그 기준보다 작은 요소들은 왼쪽으로,
큰 요소들은 오른쪽으로 재배치하는 과정을 재귀적으로 반복하는 알고리즘

### 구현개요

![](https://www.techiedelight.com/wp-content/uploads/Quicksort.png)

1. 피벗 선택: 배열의 가장 오른쪽 요소를 피벗으로 선택한다.

2. 고정 포인터 설정: 피벗보다 작은 값을 찾기 위해 왼쪽 끝에 포인터 i 를 하나 둔다

3. 이동 포인터 설정: 왼쪽끝에서 피벗을 향해 이동하는 포인터 j를 둔다.

4. 성분 교환: 이동 포인터는 현재 가리키는 요소가 피벗보다 작은 값을 갖는다면, 이동을 멈추고
   고정 포인터가 가리키는 요소위치를 변경한다.

5. 요소 교환: 포인터가 멈출 때마다, 포인터가 가리키는 요소와 피벗보다 작은 다음 요소의 위치를 교환한다.

피벗 교환: 포인터가 배열의 오른쪽 끝에 도달하면, 포인터가 가리키는 위치의 다음 요소(피벗의 바로 왼쪽 요소)와 피벗의 위치를 교환한다.

분할 및 반복: 이제 피벗은 최종 위치에 있으며, 피벗을 기준으로 배열을 두 부분으로 나눈다. 각 부분에 대해 동일한 과정을 반복하여 정렬을 완료한다.

###

### 구현

```ts
class QuickSorter {
	constructor(arr) {
		this.arr = [...arr];
	}

	private swapAndPartition(left, right) {
		// pivot: right, 배열의 가장 우측값
		const pivot = this.arr[right];

		// i: pivot보다 작은값을 이동시킬 지점
		let i = left - 1;

		// j: pivot과 원소를 비교할 포인터
		for (let j = left; j < right; j++) {
			// 값이 피벗보다 작으면, 왼쪽(i)로 보냄
			if (this.arr[j] < pivot) {
				i++;
				[this.arr[i], this.arr[j]] = [this.arr[j], this.arr[i]];
			}
		}
		// loop가 끝난후, 인덱스가 i 이하인 위치의 모든 값은 pivot보다 작은 값이 존재함

		// 피벗을 i + 1로 이동
		[this.arr[i + 1], this.arr[right]] = [this.arr[right], this.arr[i + 1]];

		// 피벗을 기준으로 양쪽으로 나눔
		return i + 1;
	}
	private sortStart(left, right) {
		if (left < right) {
			const partitioningIndex = this.swapAndPartition(left, right);

			this.sortStart(left, partitioningIndex - 1);
			this.sortStart(partitioningIndex + 1, right);
		}
	}

	sort() {
		this.sortStart(0, this.arr.length - 1);
		return this.arr;
	}
}

const quickSorter = new QuickSorter([10, 7, 8, 9, 1, 4, 2, 5, 12]);
console.log(quickSorter.sort());
```

### 시간 복잡도

평균: n \* $\log n$

퀵 정렬은 분할과 정렬을 반복하는 알고리즘이다.

이상적인 상황에서는 리스트가 계속 절반으로 나누어진다면,
n의 길이를 갖는 리스트에대해 $\log_2 n$ 번의 `분할`이 수행된다.

각 분할 사이사이마다 `정렬` n번 이하의 비교 및 교환작업이 수행된다

따라서 n \* $\log_2 n$ 의 시간복잡도를 단순화하면
n \* $\log n$ 이 된다.

최악의 경우: $n^2$

- 이미 목표 정렬 방향의 역순으로 정렬으로 정렬된 경우
  - 분할도 제대로 되지않고, n번 위치를 바꿔

5 4 3 2 `1`

=> 비교 4번 수행후 자리변경

`1` 5 4 3 2

=> 분할 및 피벗 선정

5 4 3 `2`

``

## 2.2 병합 정렬 (Merge Sort)

리스트를 절반으로 계속해서 나눈 뒤,
길이가 1인 리스트가 나오면, 마지막 분할에서 합쳐져있던 원소끼리 정렬을 수행한 후
리스트를 병합하며 위의과정을 재귀적으로 반복하는 알고리즘

![](https://www.101computing.net/wp/wp-content/uploads/Merge-Sort-Algorithm.png)

### 순서

1. 각 집합이 자료를 하나씩만 포함할때까지 절반으로 나눈다.
2. 마지막으로 나눠지기 전 합쳐져있던 대상끼리 정렬한다.
3. 다시 합친다.

### 구현

```js
// Merges two subarrays of arr[].
// First subarray is arr[l..m]
// Second subarray is arr[m+1..r]

class MergeSorter {
	constructor(arr) {
		this.arr = [...arr];
	}
	private merge(left, mid, right) {
		//mid를 기준으로 배열을 두개로 분할함
		const leftArray = this.arr.slice(left, mid + 1);
		const rightArray = this.arr.slice(mid + 1, right + 1);

		let leftPointer = 0,
			rightPointer = 0,
			originArrayPointer = left;

		//두 배열의 원소를 순서대로 비교하며 기존 배열에 저장
		while (
            leftPointer < leftArray.length &&
            rightPointer < rightArray.length
            ) {
			if (leftArray[leftPointer] <= rightArray[rightPointer]) {
				this.arr[originArrayPointer] = leftArray[leftPointer];
				leftPointer++;
			} else {
				this.arr[originArrayPointer] = rightArray[rightPointer];
				rightPointer++;
			}
			originArrayPointer++;
		}

		// 비교후 남은 배열의 원소를 기존 배열에 다시 저장

		while (leftPointer < leftArray.length) {
			this.arr[originArrayPointer] = leftArray[leftPointer];
			leftPointer++;
			originArrayPointer++;
		}

		while (rightPointer < rightArray.length) {
			this.arr[originArrayPointer] = rightArray[rightPointer];
			rightPointer++;
			originArrayPointer++;
		}
	}

	private partition(left, right) {
		if (left >= right) {
			return;
		}
		const mid = left + parseInt((right - left) / 2);
		this.partition(left, mid);
		this.partition(mid + 1, right);
		this.merge(left, mid, right);
	}

	sort() {
		this.partition(0, this.arr.length - 1);
		return this.arr;
	}
}

mergeSorter = new MergeSorter([12, 4, 7, 9, 2, 4, 5, 1]);
console.log(mergeSorter.sort());
```

### 시간복잡도

n \* $\log n$

## 2.3 힙 정렬 (Heap Sort)

배열이나 리스트를 힙으로 재구성 한후, 힙의 Root 노드를 제거 및 열거하면서
전체를 정렬하는 알고리즘

- 불안정

- Heap은 항상 우선순위가 가장높은 값을 갖는 Node를 Root Node로 배치시킴

### 순서

1. 정렬을 원하는 자료로 Heap을 만든다.

2. Heap Tree의 Root를 반복해서 제거 및 열거한다.

### 구현

```js
class HeapSorter {
	constructor(arr) {
		this.arr = [...arr];
	}

	sortStart() {
		const terminal = this.arr.length;

		// 배열을 Heap으로 재구성
		for (let i = Math.floor(terminal / 2) - 1; i >= 0; i--)
			this.heapify(terminal, i);

		// 요소를 하나 제거후 다시 Heap 구성
		for (let i = terminal - 1; i > 0; i--) {
			[this.arr[0], this.arr[i]] = [this.arr[i], this.arr[0]];
			this.heapify(i, 0);
		}
	}

	heapify(terminal, i) {
		// largest: 최댓값 좌표
		// i는 부모노드 좌표
		// left, right child
		let largest = i;
		const left = 2 * i + 1;
		const right = 2 * i + 2;

		// left child의 값이 부모보다 크면 최댓값 좌표 변경
		if (left < terminal && this.arr[left] > this.arr[largest]) {
			largest = left;
		}
		// right child의 값이 부모보다 크면 최댓값 좌표 변경
		if (right < terminal && this.arr[right] > this.arr[largest]) {
			largest = right;
		}

		// 최댓값 좌표와 부모노드가 불일치하면, 노드 위치 변경
		if (largest != i) {
			[this.arr[i], this.arr[largest]] = [this.arr[largest], this.arr[i]];

			this.heapify(terminal, largest);
		}
	}

	sort() {
		this.sortStart();
		return this.arr;
	}
}
```

### 시간복잡도

n \* $\log n$

---

# Question

1. Quick sort가 최악의 성능을 보여주는 상황을 묘사하고, 그때의 시간복잡도를 제시해주세요

---

# Answer

1. 정렬하려는 순서와 정확히 역순으로 정렬되어있고,피벗을 한쪽 끝에서 지정하는 상태

---

# References

https://www.geeksforgeeks.org/quick-sort/

https://www.geeksforgeeks.org/merge-sort/

https://www.geeksforgeeks.org/heap-sort/
