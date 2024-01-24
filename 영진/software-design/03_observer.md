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

객체들에게 일대 다 (one to many) 의존 관계를 부여하여
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

### 1. 옵저버 패턴의 두가지 핵심 객체에대해 설명해주세요.

### 2. 옵저버 패턴이 실제로 적용되는곳을 알려주세요.

---

# Answer

### 1. 옵저버 패턴의 두가지 핵심 객체와 필수구현 메서드에 대해 설명해주세요.

특정 데이터를 제공하는 Subject와, 이를 구독하여 정보를제공받는 Observer 두 가지 객체로 구성됩니다.

Subject는 Observer를 저장하는 자료구조와 Observer 등록, 해제, 데이터 변화 통지할 메서드가 필수적으로구현되어야합니다.

Observer는 Subject로부터 받은 데이터로 자신의 상태를 업데이트하는 update 메서드가 필요합니다.

### 2.옵저버 패턴이 실제로 적용되는곳을 알려주세요.

거의 모든곳에서 옵저버 패턴을 찾아볼 수있습니다.
한가지 데이터 저장소에서 나온것을 구독하여, 자신의 상태를 업데이트하는 모든것은 옵저버입니다.

- MVC 패턴으로만들어진 프로그램

데이터저장소 (Model) 의 상태가변경되면 view에 이를 알려줍니다.

- Event Driven 프로그래밍

WebSocket이나 자바스크립트의 event 기반 프로그래밍 역시 옵저버 패턴을 사용합니다.

addEventListener, removeEventListener가 대표적인 구독 / 해제 메서드입니다.

---

# References

https://refactoring.guru/design-patterns/observer
