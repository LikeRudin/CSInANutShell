# Contents

```
    1. 팩토리 메서드 패턴
        - 개요
        - 역할
        - 사용처

    2. 구현
```

---

# Details

# 1. 팩토리 메서드 패턴

## 1.1 정의

![](https://refactoring.guru/images/patterns/diagrams/factory-method/solution2-en.png?id=db5de848c1d490b835666ef54d131d46)

Factory Method Pattern

객체의 생성방법을 결정하는 생성 패턴 `creational patten`중 하나로,
특정 객체를 구현하기 전에, 객체를 생성하는 방법을 미리 정의하는 패턴

```
the factory method pattern is a creational pattern
that uses factory methods to deal with the problem of creating objects without having to specify the exact class of the object that will be created.

```

## 1.2 기능

오브젝트의 공통 Interface를 구체화 함으로써,
구체적인 Class를 만드는 로직을 노출시키지 않는다.

- `구체적인 Class들`을 사용할 또 다른 Class에서, `구체적인 Class`의 정보를 알아야하는 책임을 분리시켜준다

## 1.3 사용처

1. 공통 interface를 공유하는 일반적인 class들을 만들어 줄 때

2. class 내부에서 정확히 어떤 객체를 사용해야 할지 모를때

   - 이미 구현을 완료한 class를 사용할때에만 알 수 있는 상황

3. 객체 초기화를 위해서 비싼 비용을 지불해야 할때

---

# 2. 구현 예제

두개의 리스트가 있는 Todo List

각각 다른 기능을 가지고 있음
![](\home\yj\cs-study\CSInANutShell\영진\software-design\tododone.jpg)

## 2.1 배경 설명

Observer Class Interface

```ts
interface LiDataObserverInterface {
    // key: LocalStorage에 접근하는 데이터 Key
    // parentListId: 실제 화면에서 그려줄 TODO List 본체 ID
	constructor(key: string, parentListId: string): void;

    // Observer 관련로직
	subscribe(subject: Subject): void;
	unSubscribe(): void;

    registerLiData(key: string, value: string): void;
	update(listDataState: { [key: string]: string }): void;

    // TodoListData 수정 로직
	createLiElement(key: string, value: string): void;
	removeLiElement(key: string): void; \
	updateLiElement(key: string, value: string): void;
	}
```

그런데 이를 구현한 Class를 이용해서 실제로 Todo, Done을 관리하는
instance를 만들어 줄 때, 두 instance에서 createLiElement 메서드의
기능이 달라야 함을 발견했습니다.

![](\home\yj\cs-study\CSInANutShell\영진\software-design\tododone.jpg)

- Todo: DoneButton -> 누르면 TodoList에서 삭제
- Done: DoneButton이 없음

버튼은 모두 `createLiElement` 를 통해 생성되므로,
각 instance에 다른 같은이름의 다른 메서드를 만들어서 넣어주어야 했습니다.

사실 createLiElement는 새로운 LiData를 Observer의 data 객체에 보관하는 일과,
화면에 LiElement를 그려주는일을 모두맡고있었는데요.

그중에 LiElement를 그려주는 일에대한 메서드만 새로 만들어서 분리해서 넣어주면 되죠.

## 2.2 해결방안

- 상속
  - `createLiElement`메서드만 제거한 `LiDataObserver`를 상속받는 두 Class를 만든다
  - 각각 따로 `createLiElement`를 정의해줍니다.

하지만 단 하나의 메서드를 수정하기 위해 class를 두개나 더 만드는것은 불합리해보였습니다.

- 데코레이터 패턴

  - `createLiElement`메서드에서 DoneButton관련 로직을 삭제한다
  - 해당 메서드에 DoneButton 로직을 추가하는 데코레이터를 만든다.

자바스크립트는 데코레이터 패턴을 지원하지 않았습니다.

- 내부에서 createLiElement를 제공해줄 새로운 class / 함수를 만든다
  - `createLiElement`를 반환하는 새로운 메서드를 만든다
  - `LiDataObserver`가 입력받은 데이터의 타입에 따라, 다른 createLiElement를 주입받는다.

```js
class LiDataObserver {
  construnctor(type) {
    if (type === "todo") {
      this.listController = new TodoGenerator();
    } else {
      this.listController = new DoneGenerator();
    }
    // this.listController.createLiElement === this.createLiElement
  }
}

new TodoObserver("todo");
new TodoObserver("done");
```

그런데 위와같이 만들어줄경우, TodoObserver가
다른 listController가 무엇이 있는지 추적해야 한다는 문제점이있습니다.

새로운 Controller, 즉 새로운 기능을갖는 list를 구현할때 마다
언제나 if else 조건문에 새로운 조항을 추가해야했습니다.

### Factory Method 패턴 적용하기

- 새로운 class / 함수를 만든후, 이를 결정하여 반환하는것은 다른 class에게 위임하는것입니다.

```js
const createLiDomPainter = (stateKey) => {
  const paintDone = ({ key, value, subject, removeHandler }) => {
    const done = document.createElement("li");
    done.id = key;

    const textInput = document.createElement("input");
    textInput.value = value;

    //textInput.addEventListener(change)

    const deleteButton = document.createElement("button");

    deleteButton.innerText = "🗑";
    deleteButton.addEventListener("click", (event) => {
      const target = event.target.parentElement;
      const targetId = target.id;
      removeHandler(targetId);
    });

    done.appendChild(textInput);
    done.appendChild(deleteButton);
    return done;
  };

  const paintTodo = ({ key, value, subject, removeHandler }) => {
    const todo = document.createElement("li");
    todo.id = key;

    const textInput = document.createElement("input");
    textInput.value = value;

    const doneButton = document.createElement("button");
    doneButton.innerText = "✔";
    doneButton.addEventListener("click", (event) => {
      const target = event.target.parentElement;
      const targetId = target.id;
      removeHandler(targetId);
      subject.updateListData(DONES_STATE_KEY, targetId, value);
    });

    const deleteButton = document.createElement("button");
    deleteButton.innerText = "🗑";
    deleteButton.addEventListener("click", (event) => {
      const target = event.target.parentElement;
      const targetId = target.id;
      removeHandler(targetId);
    });

    todo.appendChild(textInput);
    todo.appendChild(doneButton);
    todo.appendChild(deleteButton);
    return todo;
  };

  switch (stateKey) {
    case TODOS_STATE_KEY:
      return paintTodo;
    case DONES_STATE_KEY:
      return paintDone;
  }
};

class LiDataObserver {
  constructor(stateKey, parentListId) {
    this.liDatas = new Map();
    this.key = stateKey;
    this.parentList = document.getElementById(parentListId);
    this.paintLi = createLiDomPainter(stateKey);
  }

  createLiElement(key, value) {
    this.liDatas.set(key, value);
    const removeHandler = (targetId) => {
      this.subject.deleteListData(this.key, targetId);
    };
    const li = this.paintLi({
      key,
      value,
      subject: this.subject,
      removeHandler,
    });
    this.parentList.appendChild(li);
  }
}
```

# Question

1. 팩토리 메서드 패턴은 디자인 패턴 분류중 어디에속하며, 어떤 때에 사용하는지 설명해주세요.

---

# Answer

1. 팩토리 메서드 패턴은 디자인 패턴 분류중 어디에속하며, 어떤 때에 사용하는지 설명해주세요.

팩토리 메서드패턴은 객체를 생성하는 생성 디자인패턴에 속합니다.
주로 여러종류의 비슷한 객체를, 사용하는 class에서 , 해당 class의 사용 시점이 되어서야
어떤 객체를 내부에서 사용할지 정 할 수있을때, 해당 class에서
필요한 객체의 종류를 식별하는 책임을 분리해주기위해사용합니다.

---

# References

https://en.wikipedia.org/wiki/Factory_method_pattern
