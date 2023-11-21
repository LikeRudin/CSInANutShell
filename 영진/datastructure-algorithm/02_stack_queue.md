# Contents

```
1. Stack

2. Queue

```

---

# Details

## 1. Stack

```
일반적으로 더미를 stack이라고 한다.


Motivation

- Last In First Out
    - 가장 먼저 들어온게 제일 먼저 처리되어야 하는곳에 사용
    - 기존 작업의 history이력을 관리하면서 새로운 요소를 처리하는 모든작업에 이용
        - 중첩된호출 함수의 정보 저장, 브라우저 뒤로가기 기록, VSCode와 같은 IDE의 작업 취소 (control z) 작업 취소 취소 (control y) 등에 이용
```

```js
export class Stack<T> {
    top: NodeForStack<T> | null;
    bottom: NodeForStack<T>| null;
    size: number;
    constructor(){
        this.bottom = null;
        this.top = null;
        this.size = 0;
    }
    push(val: T, next=null){
        const newNode = new NodeForStack(val, next);
        if(this.size ===0){
            this.top = newNode;
            this.bottom = newNode;
        } else {
            const temp = this.top;
            this.top = newNode;
            this.top.next = temp;
        }

        this.size++;
    }
    pop(){
        if (this.size === 0){
            return null
        }
        const popNode = this.top;
        if (this.top === this.bottom){
            this.bottom = null;
        }
        this.top = this.top!.next;
        this.size--;
        return popNode;
    }

}
```

# 2. queue

```
일반적으로 줄을 queue라고 한다.

Motivation

- First In First Out
    - 저장한 순서대로 데이터를 처리해야할때 사용
    - cpu 스케쥴링이나 하드웨어 인터럽트등과 같은 자원 공유 상황에서 순서관리에 사용
    - 비동기 데이터처리, 실시간 처리, BFS 탐색에 이용
```

```js
export class Queue<T> {
    head: NodeForQueue<T> |null;
    tail: NodeForQueue<T> |null;
    size: number;

    constructor(){
        this.head = null;
        this.tail = null;
        this.size = 0;
    }

    enqueue(val: T, next= null){
        const newNode = new NodeForQueue(val, next);
        if (this.size === 0){
            this.head = newNode;
            this.tail = newNode;
        } else {
        this.tail!.next = newNode}
        this.size++;
        return this
    }
    dequeue(){
        if (this.size === 0 ){
            return null;
        }
        const dequeueNode = this.head;
        this.head = this.head!.next;
        this.size--;
        return dequeueNode;
    }

}
```

---

# Question

1. Stack과 Queue의 쓰임새가 어떻게 다른지 설명하세요

---

# Answer

1. Stack과 Queue모두, 입력받은 자료를 특정 순서대로 처리하기위한 자료구조입니다.
   Stack은 가장 나중에 입력받은 데이터를 먼저처리하고, Queue는 데이터가 입력된 순서대로 처리합니다.

---

# Reference
