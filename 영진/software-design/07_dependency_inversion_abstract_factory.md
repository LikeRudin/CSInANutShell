# Contents

```

1. 의존관계 역전원칙
    - 정의
    - 의의
    - 적용 예시
    - 한계

```

---

# Details

# 1. 의존관계 역전원칙

## 1.1 정의

SOLI`D`

Dependency Inversion Principle

상위 모듈이 하위 모듈에 의존하지 않게하는 객체지향 디자인 원칙

- 추상화 수준이 더 높은 모듈의 코드가 더 낮은 모듈의 코드에 의존하는 것을 금지
  - 구현이 추상화에 의존해야함

## 1.2 의의

1. 코드의 유연성을 극대화 할수 있음

- 구체적인것이 아닌 추상적인것에 의존할수록 변경에대한 유연성이 커짐 🐱‍🏍.
  - 유연성: 코드가 변경에 쉽게 대응하는 능력

2. 상위계층 모듈의 코드를 하위계층의 코드로부터 독립적으로 구현 할 수있음

- 전통적인 경우, 정책을 결정하는 상위계층의 모듈이
  세부사항의 구현을 담당하는 하위계층에 의존하게 됨

전통적인 코드 의존구조
![](https://www.oreilly.com/api/v2/epubs/9781492077992/files/assets/f0138-01.png)

의존성 역전원칙을 적용한 구조
![](https://www.oreilly.com/api/v2/epubs/9781492077992/files/assets/f0140-01.png)

### 정책과 세부사항

|             | 정책결정                                                                         | 세부사항                                                                           |
| ----------- | -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| 정의        | 문제의 해결의 핵심 로직, 규칙 <br> 특정 기능을 제공하는 방법에대한 고수준의 로직 | 정책결정을 구현하는데에 필요한 구체적인 방법 <br> 알고리즘,자료구조, 저수준의 로직 |
| 추상화 수준 | 높음 ↑                                                                           | 낮음 ↓                                                                             |
| 변경 빈도   | 낮음 ↓                                                                           | 높음 ↑                                                                             |
| 예시        | 로그인, Todolist 데이터 관리                                                     | 인증 검증로직, 데이터 저장방식,                                                    |

## 1.3 적용 예시

### 전통적인 경우의 예시

- 상위 Class내부에서 하위 Class의 `Instance`를 직접사용하는경우
  - `Inferface`가아니라 구체적인 구현에 의존

`LiDataObserver` class는 `listController`라는속성을 통해
Li Element에대한 Dom 컨트롤을 수행

- `listController`: Li Element를 화면에서 그리거나 제거하는 책임을 담당

```js
class LiDataObserver {
  construnctor() {
    this.listController = new TodoListController();
  }
  createLiElement() {
    // TodoListController를 직접 참조
    this.listController.paintLiElement();
  }
}
```

-`LiDataObserver`: 정책을 결정

    - 객체의 메서드를 어떻게 사용하는지 결정

- `TodoGenerator`: 세부사항을 결정

  - 메서드 내부 로직에대한 책임을 갖고있음

### 의존성 역전 원칙 사용의 예시

상위 class는 `IListController`라는 추상적인 `interface`에 의존한다.

```js
class LiDataObserver {
  listController: IListController;

  constructor(type) {
    this.listController = createListController(type);
  }
}
```

- 이제 LiDataObserver의 코드는 `IListController`라는 추상 interface에 의존하고 있음

  - 의존 대상이 실질적인 구현체(`TodoListController`)가아님

- TodoListController는 `IListController`라는 정해진 규격에 맞게 만들어야함

### 제작 순서

1. 하위 모듈에대한 interface를 선 제작

```ts

interface IListController {
    createTodoElement(key, text) => void;
    removeTodoElement(key) => void;
    updateTodoElement(key, text) => void;
}
```

2. 하위 모듈의 interface에 맞는 모듈들을 제작

```ts
class TodoListController implements IListController {
  createTodoElement(key, text) {}
  removeTodoElement(key) {}
  updateTodoElement(key, text) {}
}

class DoneListController implements IListController {
  createTodoElement(key, text) {}
  removeTodoElement(key) {}
  updateTodoElement(key, text) {}
}

class DoingListController implements IListController {
  createTodoElement(key, text) {}
  removeTodoElement(key) {}
  updateTodoElement(key, text) {}
}
```

3. 기존 LiDataObserver에 instance를 제공하는 함수작성

```js
const createListController(type): IListController => {
  switch(type){
    case "TODO":
      return new TodoListController();
    case "DONE":
      return new DoneListController();
    case "DOING":
      return new DoingListContoller();
  }
}

class LiDataObserver {
  listController: IListController;

  constructor(type) {
    this.listController = createListController(type);
  }
}
```

### 의의

상위 모듈인 LiDataObserver가 하위 모듈인
IListController의 구현체들에 맞춰 코드를 변경할 필요가 없음

- 더 안정적인 코드

결합이 조금더 느슨해졌으므로, 조금더 자유로운 순서로 프로그래밍을 할 수있음

1. 데이터 제공모듈 (subject)
2. 구독 모듈(observer)
3. 데이터 활용 모듈 (ListController)

구독 모듈이 ListController의 구체적인 구현에 의존할 때에는
2번과 3번의 구현을 함께 해야 했지만, `Interface`에 의존할때에는 분리해서 할 수도 있음

---

# Question

1. 구현체가 아닌 추상적인 interface에 의존하면 어떤 장점이 있나요?

---

# Answer

1. 특정 class가 다른 class의 구현체가 아닌 추상적인 interface에 의존하면 어떤 장점이 있나요?

좀더 느슨하게 결합이 되므로, 유연성이 증가합니다.

의존대상인 모듈의 interface만 변경되지않는다면, 구현 내용이 어떻게 바뀌어도 상관이없으므로,
유연하게 구현체를 변경할 수있고, 그러므로, 작업을 분배하여 협업하기에도 유리합니다.

---

# References

```

```
