# Contents

```
    1. AOP 개요
        - 정의
        - 주요 용어
        - 예시

    2. decorator 디자인 패턴
        - 사전적 정의 /CS 디자인 정의
        - 디자인 패턴적 의의
            - 기능의 확장: OCP
            - 기능의 분리: AOP
        - 타입스크립트 예제

```

---

# Details

# 1. AOP 개요

## 1.1 AOP의 정의

관점지향 프로그래밍 Aspect Oriented Programming

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F0c9aec3e-f216-43aa-b040-9196b9c4e950%2FUntitled.png&blockId=0feac740-7690-4d29-bcdb-48abc9b40fbe)

여러 모듈이나 클래스에 걸쳐 나타나는 관심사인 `횡단 관심사`의 분리를 허용함으로써,
모듈성과 재사용성, 유지보수성을 높여주는 프로그래밍 철학

## 1.2 주요 용어

### 1.2.1 Cross-Cutting Concerns

횡단 관심사

공통으로 적용되는 관심사

- 서로 다른 비즈니스 로직들이 공통으로 필요로 하는 로직

![](https://i.stack.imgur.com/0xO3n.jpg)

### 1.2.2 Advice

- 횡단관심사로 분리할 수있는 부분의 기능

  - 대표적으로 로깅, 보안, 정보 타입 변환 등

- 타겟에 대한 실행 시점을 다음과 같이 분류
  - Before, AfterReturning, AfterThrowning, After, Around

### 1.2.3 Target

- Advice 적용 대상 (객체)

Join Point

- 부가기능을 적용하는 지점 혹은 수단 (메서드 호출, 객체 생성 등)

Point cut

- 실제적용될지점 - 코드상의 지점

부가기능-인프라 로직을 구현하는데에 많이 사용한다.

중복코드를 줄일 수있다.
비즈니스 로직에 집중할 수있다.

## 1.3 예시

```js
const app = express();
const logger = morgan("dev");

app.set("view engine", "pug");
app.set("views", process.cwd() + `/src/views`);

//로깅
app.use(logger);

// Request data 본문을 파싱
app.use(express.urlencoded({ extended: true }));
app.use(express.json());

app.use(
	session({
		secret: process.env.COOKIE_SECRET,
		resave: false,
		saveUninitialized: false,
		store: MongoStore.create({ mongoUrl: process.env.DB_URL }),
	})
);

app.use(flash());
app.use(localsMiddleware);

app.use("/uploads", express.static("uploads"));
app.use("/static", express.static("assets"));

app.use("/", rootRouter);
app.use("/videos", videoRouter);
app.use("/users", userRouter);
app.use("/api", apiRouter);

export default app;
```

| advice                                                                                   | joint point                  | target       | point cut                    |
| ---------------------------------------------------------------------------------------- | ---------------------------- | ------------ | ---------------------------- |
| logger </br> 부가기능: app 객체를 거치는 서버 함수의 실행에 대한 정보를 console에 출력   | 서버 함수 실행               | app 객체     | 서버함수 실행지점            |
| express.urlencoded() <br> 부가기능: content-type이 x-www-form-urlencoded인 데이터를 파싱 | request 요청을 해석하는 부분 | request body | request가 들어오는 모든 경로 |
| express.json <br> 부가기능: content-type이 application/json 인 데이터를 파싱             | request 요청을 해석하는 부분 | request body | request가 들어오는 모든경로  |
| app.use("/api", apiRouter) <br> "/api"로시작하는 라우팅경로일경우, apiRouter 실행        | request 요청                 | app 객체     | "/api" 경로                  |

### logger를 api로 옮긴경우

```js
import express from "express";
import { protectorMiddleware } from "../middlewares";
import {
	deleteComment,
	registerView,
	uploadComment,
} from "../controllers/videoController";

const apiRouter = express.Router();

const logger = morgan("dev");
apiRouter.use(logger);

apiRouter.post("/videos/:id([0-9a-f]{24})/view", registerView);
apiRouter.post(
	"/videos/:id([0-9a-f]{24})/comment",
	protectorMiddleware,
	uploadComment
);
apiRouter.delete(
	"/videos/:id([0-9a-f]{24})/comment/delete",
	protectorMiddleware,
	deleteComment
);

export default apiRouter;
```

| advice                                                                                      | joint point                        | target        | point cut   |
| ------------------------------------------------------------------------------------------- | ---------------------------------- | ------------- | ----------- |
| apiRouter.use(logger) <br> apiRouter를 거치는 서버 함수의 실행에 대한 정보를 console에 출력 | apiRouter를 경유하는 서버함수 실행 | apiRoute 객체 | "/api" 경로 |

# 2. Decorator 디자인 패턴

## 2.1 Decorator의 정의

### 사전적 정의

```
Decorate (v) 장식하다, 모양내다, 꾸미다

Decorator (n) 장식, 장식하는 사람
```

### In CS

데코레이터 패턴

객체에 추가적인 책임을 동적으로 부여하는 디자인 패턴
기능의 확장을 위해 사용하는 구조 (Structural)디자인 패턴이다.

subclass를 대체할 수 있다.

```
The Decorator Pattern

attaches additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.
```

## 2.2 디자인 패턴적 의의

### 2.2.1 기능의 확장: OCP

데코레이터는 기존 클래스를 변경하지 않고 기능을 확장시킴으로써 개방-폐쇄 원칙(OCP)을 만족시킴

### 상속(inheritance)보다 나은점

- 데코레이터는 동적으로 속성을 변경
- 실행중에도 유연하게 class의 속성을 변경할 수있음

- `class explosion`을 배제할 수있음
  ![](https://blog.lukaszewski.it/wp-content/uploads/2013/03/class-explosion.png)

  - `class explosion`: 가능한 기능의 조합이 많이필요할때, 엄청많은 class를 새로 만드는것

  - 데코레이터를 사용하는 경우
    ![](https://blog.lukaszewski.it/wp-content/uploads/2013/03/starbuzz-decorator.png)

### 2.2.2 기능의 분리: AOP

서로 다른 비즈니스 로직에서 공통적으로 요구되는
부분을 각 객체나 함수에서 내부에서 분리하여 사용할 수 있게 함

이는 횡단 관심사를 분리하는 관점 지향 프로그래밍(AOP) 철학과 일치

## 2.3 타입스크립트 예제

### TS 5.0

- 다음 Class의 메서드 실행여부를 console에 출력하고싶다.

```ts
class Person {
	name: string;
	constructor(name: string) {
		this.name = name;
	}

	greet() {
		console.log(`Hello, my name is ${this.name}.`);
	}
}

const p = new Person("Ron");
p.greet();
```

- 메서드에 직접 console.log로 log를 출력하는경우

```ts
class Person {
	name: string;
	constructor(name: string) {
		this.name = name;
	}

	greet() {
		console.log("LOG: Entering method.");

		console.log(`Hello, my name is ${this.name}.`);

		console.log("LOG: Exiting method.");
	}
}
```

- 데코레이터 메서드를 만든다.

```ts
function loggedMethod(originalMethod: any, _context: any) {
	function replacementMethod(this: any, ...args: any[]) {
		console.log("LOG: Entering method.");
		const result = originalMethod.call(this, ...args);
		console.log("LOG: Exiting method.");
		return result;
	}

	return replacementMethod;
}
```

- 데코레이터를 적용시켜준 모습

```ts
class Person {
	name: string;
	constructor(name: string) {
		this.name = name;
	}

	@loggedMethod
	greet() {
		console.log(`Hello, my name is ${this.name}.`);
	}
}

const p = new Person("Ron");
p.greet();

// Output:
//
//   LOG: Entering method.
//   Hello, my name is Ron.
//   LOG: Exiting method.
```

### Nest JS 라이브러리

```ts
import { Controller, Get } from "@nestjs/common";

@Controller("cats") // 이 데코레이터의 사용만으로, 하단 class가 /cats 라우팅 기능을 얻음
export class CatsController {
	@Get() //Get '/cats/' 요청에는 아래 메서드 실행
	findAll(): string {
		return "This action returns all cats";
	}

	@Get("docs") // Get '/cats/docs' 요청에는 아래 메서드 실행
	@Redirect("https://docs.nestjs.com", 302)
	// version param을 자동으로 request query로 만들어줌
	getDocs(@Query("version") version) {
		if (version && version === "5") {
			return { url: "https://docs.nestjs.com/v5/" };
		}
	}
	// express였디면 이런식으로 꺼내야함
	// (req, res) => { const version = req.query }
}
```

### TS 예제

```ts
const classDecorator = (constructor: Function) => {
	console.log("Class Decorator has 1 arguments");
	console.log(constructor);
	console.log("Class Decorator  finishied");
};

const propertyDecorator = (classPrototype: Object, propertyName: string) => {
	console.log("Property Decorator has 2 arguments");
	console.log(classPrototype);
	console.log(propertyName);
	console.log("propertyDecorator finished");
};

const methodDecorator = (
	classPrototype: Object,
	methodName: string,
	methodDescriptor: PropertyDescriptor
) => {
	console.log("method Decorator has 3 arguments");
	console.log(classPrototype);
	console.log(methodName);
	console.log(methodDescriptor);
	console.log("Method Decorator finishied");
};

const parameterDecorator = (
	constructor: Object,
	methodName: string,
	paramindex: number
) => {
	console.log("Parameter Decorator has 3 arguments");
	console.log(constructor);
	console.log(methodName);
	console.log(paramindex);
	console.log("Parameter Decorator finishied");
};

const accessorDecorator(classPrototype: Object, propertyName: string) {
	console.log("Accessor Decorator 2 arguments");
	console.log(classPrototype);
	console.log(propertyName);
	console.log("Accessor finishied");
}

@classDec
class ExClass {
	@propertyDec
	exProperty = "property";

	@methodDec
	exMethod(@parameterDec word: string) {
		console.log("Method " + word);
	}

	@accessorDec
	get exAccessor() {
		return "value";
	}
}
```

# Question

1. Decorator와 Subclass는 모두 구조 디자인 패턴으로 기능 확장을 위해 사용됩니다. 각각 어떤 상황에서 더 유용할까요?

---

# Answer

1. 여러기능을 조합해서 사용해야 할 경우, Decorator가 더 적합합니다.만약 각 기능을 갖고있는 subclass를 일일이 생성하는 경우에는 경우에는 너무많은 class- `class explosion` 을 겪을 수 있습니다.

하지만, Decorator는 컴파일 시점이 아닌 런타임에 적용되므로, 안정적인 구조가 필요한 경우에는 Subclass를 만드는 것이 좋습니다.

---

# References

- https://www.youtube.com/watch?v=Hm0w_9ngDpM
- https://devblogs.microsoft.com/typescript/announcing-typescript-5-0/#decorators
- https://blog.lukaszewski.it/2013/03/31/design-patterns-decorator/
