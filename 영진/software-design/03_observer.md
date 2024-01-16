# Contents

```
    1. Observer Design Pattern 개요
        - 정의
        - Interface
        - 동작 프로세스

    2. 예시

```

---

# Details

# 1. Observer Design Pattern 개요

## 1.1 정의

일대 다 (one to many) 의존 관계를 가진 객체들 간에,
한 객체의 상태 변화 시 다른 객체들의 상태도 함께 업데이트하는 디자인 패턴

의존의 대상인 하나의 객체를 `Subject`, 나머지를 `Observer` 로 분류하여
`Subject`의 상태가 변화할때마다, `Observer`들의 상태도 함께 업데이트 해준다.

- `Observer` 역할을 할 수있게 구현된 객체는 꼭 `Subject`의 정보를 받아올 필요는없음
- `Subject`는 자신에게 의존하는 `Observer`를 등록(`Register`) 및 탈퇴 (`remove`) 시킬 수있음

## 1.2 Interface

![](https://refactoring.guru/images/patterns/diagrams/observer/solution2-en.png)

```ts
interface Observer {
    update: () => void;
}

interface Subject {
    registeredObservers: Observer[]
    registerObserver (observer: Observer) => void;
    removeObserver (observer: Observer) => void;
    notifyObservers() => void;
}
```

## 1.3 동작 프로세스

### 데이터 업데이트시

- Subject.`notifyObservers()`
  - Subject.`registeredObject`에 들어있는 `Observer`의 배열을 순회
  - Observer.`update()`

### 옵저버 등록시

- Subject.`registerObject(observer)`
  - Subject.`registeredObject` 배열에 입력받은 `Observer`를 등록

### 옵저버 해제시

- Subject.`removeObserver(observer)`
  - `Subject.registerdObject`에서 `Observer` 제거

---

# Question

---

# Answer

---

# References

https://refactoring.guru/design-patterns/observer
