# Contents

```
1. Array

2. ArrayList

3. LinkedList

4. LinkedLisst vs ArrayList vs Array

```

---

# Details

1. Array

## 1. Array?

```
메모리에 연속적으로 할당되어있는 자료의 집합.

Motivation:

- 같은 타입의 데이터를 연속된 데이터 공간에 나란히 저장하여
데이터 저장에 필요한 공간 및 저장된 데이터의 위치를 쉽게 계산할 수 있음.
```

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20230726162247/Array-data-structure.png)

- 선언할때에 크기와 데이터 타입을 지정해주어야 함

```c
int arr[5];     // 타입: int 길이: 5
char arr[10];    // 타입: char 길이: 10
float arr[20];  //타입: float int 길이: 20
```

- 하지만 자바스크립트나 파이썬과 같은 동적 스크립트 언어들은 그런거 없음

```js
let arr = [10, 20, 30]; // 정수
let arr2 = ["c", "d", "e"]; // 문자
let arr3 = [28.5, 36.5, 40.2]; // 부동소수
```

- 검색, 끝부분에서의 삽입, 삭제등에 상수시간이 소요 O(1)
- 자료의 중간에서 삽입, 삭제시 선형시간 소요O(n)

## 2. Array List

```
동적으로 크기가 변하는 Array.

Motivation:

- Array의 길이 관리책임을 개발자에게서 프로그래밍 언어로 넘김으로써,
런타임에서 자료구조에 저장될 데이터의 양을 고려할 필요가 없어짐
```

- 커지는 비율(resizing factor)은 언어에따라 다르며, JAVA의 경우 2배씩 커짐

- 검색, 맨앞이나 맨뒤에 자료 삽입/삭제에 상수시간이 소요 O(1)

  - 최악의 경우에는 O(n)
  - ArrayList가 가득찼을때, 자료구조의 크기를 2배로 증가시키므로, n번의 삽입작업이 이루어짐

- 맨앞이나 맨뒤가 아닌 위치에 자료 삽입/삭제에는 선형시간이 소요O(n)

## 3. LinkedList

```
연속된 node를 연결한 자료구조
각 node는 데이터와 다음 node를 가리키는 pointer로 구성되어있음

motivation:

맨앞이나 뒤에서 데이터의 삭제, 삽입을 상수시간내에 수행하기위해 고안됨
```

![](https://camo.githubusercontent.com/d73dafeb8e51815cc72c5cd69f6b9e45591bf3eef7abdbeaad6d5606aecf9440/68747470733a2f2f6d656469612e6765656b73666f726765656b732e6f72672f77702d636f6e74656e742f75706c6f6164732f32303232303831363134343432352f4c4c64726177696f2e706e67)

일반적으로 head, tail 그리고 length 프로퍼티를 가짐

- 맨 앞이나 맨뒤의 자료삭제, 삽입, 접근은 상수시간 O(1), 다른 위치에서는 O(n)이 소요

```ts
export class SinglyLinkedList<T> implements LinkedList<T> {
	head: NodeForSinglyLinkedList<T> | null;
	tail: NodeForSinglyLinkedList<T> | null;
	length: number;

	constructor() {
		this.head = null;
		this.tail = null;
		this.length = 0;
	}

	push(val: T, next = null) {
		const newNode = new NodeForSinglyLinkedList(val, next);
		if (this.length === 0) {
			this.head = newNode;
			this.tail = newNode;
		} else {
			this.tail!.next = newNode;
			this.tail = newNode;
		}
		if (this.tail.next) {
			let newTail = newNode;
			while (newTail.next) {
				newTail = newTail.next;
				this.length++;
			}
			this.tail = newTail;
		}
		this.length++;
		return this;
	}
	pop() {
		if (this.length === 0) {
			return undefined;
		}

		let pop = this.head;
		let newTail = pop;

		while (pop!.next) {
			newTail = pop;
			pop = pop!.next;
		}

		newTail!.next = null;
		this.tail = newTail;
		this.length--;

		if (this.length === 0) {
			this.tail = null;
			this.head = null;
		}
		return pop;
	}

	shift() {
		if (this.length === 0) {
			return undefined;
		}
		const shift = this.head;
		this.head = shift!.next;
		this.length--;

		if (this.length === 0) {
			this.tail = null;
		}
		return shift;
	}

	unshift(val: T, next = null) {
		const unshift = new NodeForSinglyLinkedList(val, next);

		if (this.length === 0) {
			this.head = unshift;
			this.tail = unshift;
		} else {
			unshift.next = this.head;
			this.head = unshift;
		}
		this.length++;
		return this;
	}

	get(index: number): NodeStructure<T> | null {
		if (index < 0 || index >= this.length || this.length === 0) {
			return null;
		}

		let get = this.head;
		for (let i = 0; i < index; i++) {
			get = get!.next;
		}
		return get;
	}

	set(index: number, val: T) {
		const set = this.get(index);
		if (!set) return false;

		set!.val = val;
		return true;
	}

	insert(index: number, val: T) {
		if (index < 0 || index > this.length) {
			return false;
		}
		if (index === this.length) {
			this.push(val);
			return true;
		}
		if (index === 0) {
			this.unshift(val);
			return true;
		}

		const prev = this.get(index - 1);
		const next = this.get(index);
		const newNode = new NodeForSinglyLinkedList(val, next);

		prev!.next = newNode;
		this.length++;

		return true;
	}

	remove(index: number) {
		if (index < 0 || index >= this.length) {
			return undefined;
		}
		if (index === this.length - 1) {
			return this.pop();
		}
		if (index === 0) {
			return this.shift();
		}
		const prev = this.get(index - 1);
		const remove = this.get(index);
		const next = this.get(index + 1);

		prev!.next = next;
		this.length--;
		return remove;
	}

	reverse() {
		let node = this.head;
		this.head = this.tail;
		this.tail = node;
		let prev = null;

		while (node !== null) {
			let next = node.next;
			[node!.next, prev, node] = [prev, node, next];
		}

		return this;
	}
}
```

## Array vs ArrayList vs LinkedList 비교

| 속성          | linked-List        | Array                 | ArrayList                                                                                 |
| ------------- | ------------------ | --------------------- | ----------------------------------------------------------------------------------------- |
| 인덱스        | X                  | O                     | O                                                                                         |
| 임의접근      | X                  | 인덱스                | 인덱스                                                                                    |
| 노드 이동방식 | 노드의 pointer     | 인덱스                | 인덱스                                                                                    |
| 크기          | 가변               | 고정                  | 가변                                                                                      |
| 장점          | 삭제와 삽입이 빠름 | 인덱스로인한 빠른접근 | Resizing factor에 의해 크기 상승률이 결정 <br> 언어별로 다르며, 자바의경우는 2배씩 상승함 |

# Question

1. Array에는 데이터들이 저장될떄 타입은 어떻게 저장되나요?, 그에대한 장점 혹은 단점 은 무엇인가요?

2. Array는 인덱스를 통해 위치와 관계없이 모든 데이터에 상수시간에 접근가능한데, LinkedList를 사용하는 이유가 무엇인가요?

---

# Answer

1. 일반적으로 정적 타입 언어에서 배열(Array)은 동일한 타입의 데이터들을 저장합니다.
   같은 타입의 데이터는 유사한 메모리 공간을 차지하므로, 메모리 계산 및 사용을 좀더 효율적으로 할 수있습니다.

2. LinkedList는 데이터의 삽입 및 삭제에서 Array와 구분되는 장점을 가지고있기 때문입니다.
   Array는 데이터를 메모리상의 연속된 공간에 저장하므로, Array의 중간에서 데이터를 삭제하거나 삽입할때에, 절반의 노드들을 움직여야야합니다. 하지만 LinkedList는 두개의 pointer 값만 변경하는것으로 중간위치의 데이터 삽입 및 삭제가 훨씬 효율적입니다.

---

# Reference

https://www.geeksforgeeks.org/array-data-structure/
