# Contents

```
    1. Trie 개요
        - 정의

    2. HashTable과 비교

    3. 구현

```

---

# Details

# 1. Trie 개요

긴 문자열(string)을 빠르게 검색할 수 있도록 고안된 트리

- 하나의 노드에 하나의 알파벳을 삽입

- 알파벳의 문자열 구성 순서대로 부모-자식관계를 이룸

leaf Node는 각 단어가 끝나는 지점으로, 단어의 완성을 의미함

- 특별한 타입의 child Node로 구성하거나, booleanFlag를 삽입함

```js
{
  value: "a";
  isTermiator: false;
}
```

![](https://media.geeksforgeeks.org/wp-content/uploads/20220828232752/Triedatastructure1.png)
![](https://camo.githubusercontent.com/7024b55e64516062054e9b5bccf35dc72d5e7a4cca88c8f57810804b955cb849/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323433353445333335383333413743463137)

- n-ary tree의 일종

### N-ary Tree

각 Node가 N개 이하의 자식을 가질수 있는 Tree

- Binary Tree는 2를 의미하는 접두사 Bi가 붙은것

---

# 2. HashTable과의 비교

| 대상                    | Trie                         | Hash Table                    |
| ----------------------- | ---------------------------- | ----------------------------- |
| 검색 속도               | O(Log(K)) (K: string의 길이) | O(Log(1))                     |
| 키 충돌                 | 없음                         | 있음                          |
| 단일 키 다중 값         | O                            | O (Separate Chaining)         |
| 추가 도구               | X                            | Hash Function                 |
| 데이터 검색 속도        | 때때로 매우 느림             | 일반적으로 빠름               |
| 키의 문자열 표현 복잡성 | 복잡 (예: 부동 소수점)       | 간단                          |
| 부위선별검색            | O                            | X                             |
| 공간사용 정도           | 거의 항상많음                | 적음                          |
| 사용하는곳              | 오타 교정프로그램, 검색엔진  | 데이터를 저장하는 거의 모든곳 |

# 3. 구현

```js
const ALPHABET_SIZE = 26;

class TrieNode {
  constructor() {
    this.isTerminator = false;
    this.children = new Array(ALPHABET_SIZE).fill(null);
  }
}

class Trie {
  constructor() {
    this.root = new TrieNode();
  }

  insert(key) {
    const length = key.length;

    let pCrawl = this.root;
    for (let level = 0; level < length; level++) {
      // unicode 16- 숫자로변환
      const index = key[level].charCodeAt(0) - "a".charCodeAt(0);

      if (pCrawl.children[index] === null) {
        pCrawl.children[index] = new TrieNode();
      }
      pCrawl = pCrawl.children[index];
    }
    pCrawl.isTerminator = true;
  }

  search(key) {
    const length = key.length;
    let pCrawl = this.root;

    for (let level = 0; level < length; level++) {
      const index = key[level].charCodeAt(0) - "a".charCodeAt(0);

      if (pCrawl.children[index] == null) {
        return false;
      }
      pCrawl = pCrawl.children[index];
    }
    return pCrawl.isTerminator;
  }
}
```

# Question

Trie와 HashTable은 둘다 단어를 저장하는데에 사용될 수있습니다.
다음 질문에 답해주세요

1. 검색 작업에 있어서 Trie가 어떤점에서 HashTable보다 더 나은 성능을 갖나요?

2. 데이터를 저장할떄, HashTable보다 Trie자료구조가 갖는 이점은 무엇인가요?

---

# Answer

1. 검색 작업에 있어서 Trie가 어떤점에서 HashTable보다 더 나은 성능을 갖나요?

Trie는 저장된 단어의 일부분을 검색하는데에 특화되어있습니다.

특정 접두사를 갖는단어를 한번에 검색하기 편해서 Prefix Tree라고 부르기도하며,
검색엔진의 검색어 자동완성등을 구현하는데에 아주 적합합니다.

2.  데이터를 저장할떄, HashTable보다 Trie가 갖는 자체적인 이점은 무엇인가요?

Trie는 일반 Tree와 같이, 동적으로 Node를 추가하기때문에, 저장 공간의 한계가 정해져있지 않습니다.
또 Hash Collision등과 같이 중요하게 처리해야하는 예외상황이 없습니다.

---

# References

https://www.geeksforgeeks.org/generic-treesn-array-trees/

Cracking the coding test 105p: Tries(Prefix Trees)
