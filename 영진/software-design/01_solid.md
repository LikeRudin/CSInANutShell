# Contents

```
    1. SOLID 개요
        - SRP
        - OCP
        - LSP
        - ISP
        - DIP
```

---

# Details

# 1. SRP: 단일책임 원칙

Single Responsibility Principle

class를 `수정하는 이유`는 오직 한 가지여야 합니다.

class가 여러 가지 책임을 갖게 된다면
책임별로 변경 사항이 생깁니다.

그러므로 class가 한 가지 책임을 갖게 해야 합니다.

## 그런데 잠깐

이 원칙은 class에만 적용되는것이 아님

변수와 함수, 그리고 객체와 class, Module까지,
범위가 다를뿐 SRP를 지켜줌

- Variable

  - 하나의 변수에 여러가지 다른목적의 값들을 돌아가며 넣어주는것은 좋지않음
  - number를 담았다가 string를담고, 다시 boolean을 담는것은 자제할것.

- Function

  - 한가지 일을 하는게 좋음

- Object

  - 함수보다는 커다랗게 `한가지 기능`에 대한 일을 하는것이 좋음
  - ex)Date 객체가 날씨정보또한 알려주는것은 바람직하지 않음

- Module

  - 한가지 개념과 관련된 일을 하는것이 좋음
  - 인증인가/ 유저/ 환경설정 등등 개념별로 별도의 Module을 만드는것이 좋음

# 2. OCP: 개방폐쇄 원칙

코드는 확장에는 개방적이고, 수정에는 폐쇄적이어야 함

Function이나 class를 생성해서 사용하면, 이에 의존하는 여러 로직이 생김

- 개발자가 계속바뀌는 기업의 코드에서, 특정 class나 Function을 함부로 수정할시 재앙이 일어날 수있음

따라서 기존 로직은 그대로 두면서 기능을 추가하기 용이한 방식으로 코드를 만들어야함

OCP를 만족시키는 도구로
데코레이터(Decorator), 서브 클래스(SubClass) 등을 사용할 수있음

# 3. LSP: 리스코프 치환법칙

Liskov Substitution Principle

부모객체를 상속받는 하위 객체가 있다면,
부모객체를 하위객체로 바꾸어도 이전과 같은 기능을 실행해야 함

우리는 외부라이브러리 class의 동작을 커스터마이징할때,
해당 class를 상속해서 자식 class를 만듬

# 4. ISP: 인터페이스 분리법칙

Interface Segregation Principle

메서드의 입력값 타입은 해당 입력을 위해 별도로 정의되어야 함

- 다른 메서드의 반환 타입을 입력 타입으로 사용하는 것은 바람직하지 않음

클래스가 사용하지 않는 의존성을 가져서는 안됨

- 겹치는 요소가 있다고 해서 무조건 상속받아서는 안됨

### 타입스크립트 Type

사용하지 않는 의존성을 제거하는 도구, Utility Type을 제공함

```TS
interface Todo {
  title: string;
  description: string;
  completed: boolean;
  createdAt: number;
}

type TodoPreview = Omit<Todo, "description">;
```

# 5. DIP: 의존성 역전법칙

Dependency Inversion Principle

추상화 수준이 높은 모듈이, 더 구체적인 모듈에 의존해선 안됨.
어떤 모듈이던, 상속받는 모듈은 다른 구현모듈이 아닌 추상모듈이어야 함

### NestJS 예제

상속을 받지않고, 주입을 받는 모습

```TS
import { Controller, Get, Post, Body } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';
import { CatsService } from './cats.service';
import { Cat } from './interfaces/cat.interface';

@Controller('cats') // '/cats' request 라우팅
export class CatsController {
  constructor(private catsService: CatsService) {}

  @Post()
  async create(@Body() createCatDto: CreateCatDto) {
    this.catsService.create(createCatDto);
  }

  @Get()
  async findAll(): Promise<Cat[]> {
    return this.catsService.findAll();
  }
}
```

CatsController는 CatsService의 메서드를 사용하나,
상속은 받지 않았음

CatsController 내부에 CatsService의 instance를 넣어서 사용중

- 이를 의존성 주입`Dependency Injection` 이라고 함

---

# Question

1. OCP (Open-Closed Principle)에 대해 설명하고, 이를 어떻게 준수할 수있는지 알려주세요.

2. DIP (Dependency Inversion Principle)에 대해 설명해주세요.
   그리고 A클래스가 클래스B의 로직을 사용해야하는 경우에, 어떻게 DIP를 준수할 수있는 지에대해 설명해주세요.

---

# Answer

1.

OCP(Open-Closed Principle)는 class가 수정에는 닫혀 있고 확장에는 열려 있어야 한다는 원칙입니다.
새로운 기능 추가 시 기존 class는 그대로 유지하면서, 그 class를 상속받는 새로운 class를 생성하거나 decorator 패턴을 통해 class의 기능을 동적으로 변경함으로써 이 원칙을 준수할 수 있습니다.

2.

사용해야하는 로직을 가진 class를 상속하지않고, 해당 class의 instance를 넣어줌으로써,
의존성을 제어할 수있습니다.
이를 의존성 주입(DI: Dependency Injection) 이라 합니다.

# References
