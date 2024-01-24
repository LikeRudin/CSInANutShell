# Contents

```
    1. 객체지향 프로그래밍 개요
        - 정의
        - 왜 필요한가?

    2. 객체지향의 4가지 컨셉
        - 캡슐화
        - 상속
        - 추상화
        - 다형성

```

---

# Details

# 1. 객체지향 프로그래밍 개요

## 1.1 정의

객체지향 프로그래밍

Object oriented programming

프로그래밍을 함수나 로직등으로 생각하지않고,
`특정 데이터와, 그 데이터에대해 실행되는 함수들을 묶은 덩어리`의 집합으로 생각하는것

- 프로그램을 `객체`의 집합으로 생각하는것

```
computer programming model that organizes software design around data, or objects, rather than functions and logic. An object can be defined as a data field that has unique attributes and behavior.

The main aim of OOP is to bind together the data and the functions that operate on them so that no other part of the code can access this data except that function
```

## 1.2 객체지향 프로그래밍이 왜 필요한가?

- 함수를 만드는것과 같은 이유

DRY! Dont Repeat yourself 원칙을 지켜서
같은 로직의 코드를 반복 작성하는 것 을 줄이고,
코드를 간편하게 재사용 하기 위해서이다.

### 예시

```python

# 개체를 나타내는 딕셔너리
sky = {
	"name" : 'skynet'
	"level": 99999,
	"team": "destory_mankind_on_earth"
}

# 소개 함수
def introduce_person(person):
	name = person["name"]
	team = person["team"]
	print(f"Hello, My name is {skynet} and I play for {team}")
```

위 코드에선 `sky` 딕셔너리 key 값과 (name, team)
`introduce_person` 함수가 서로 개념적으로 연결되어있다.

### 딕셔너리를 추가하는 방법

현재 `introduce_person` 함수를 사용하는 데이터는 `sky` 데이터 하나뿐이지만
만약 더 많은 데이터가 필요한 경우에는 어떻게 해야할까?

- 단순히 같은 형태의 딕셔너리를 만들면 된다.

```python
bee = {
		"name" = "bee",
		"level" 25000,
		"team": "human_resistance"
}
```

하지만 이렇게 모든 데이터를 새로 만드는것은 여러모로 피곤한일이다.

- 코드량이 늘어난다.
- 예상치못한 오타로인해 버그가 생길수 있다.

그러므로 다음과 같은 질문에 도달한다.

### Q: 언제나 `introduce_person` 에 사용할수있는 데이터(딕셔너리)를 어떻게 찍어낼 수있을까?

### A: 딕셔너리를 생성하는 Factory 함수를 만들자

```python
def create_person(name, level, team):
	return {
		"name": name,
		"level": level,
		"team": team
	}

sky = create_person("skynet", 99999, "destory_mankind_on_earth")
bee = create_person("bee", 25000, "human_resistance")

def introduce_person(person):
	name = person["name"]
	team = person["team"]
	print(f"Hello, My name is {skynet} and I play for {team}")
```

이제 `sky`와 `bee` 처럼 `create_person`에 의해 생성된 딕셔너리들은
`introduce_person` 함수를 사용할때 아무런 문제가 생기지 않을것이다.

이제 `team`별로 소속된 `person` 딕셔너리들을
분류하여 사용해보자.

### Q: 여기서 동일한 team별로 데이터를 정리 하려면 어떻게할까?

### A: 팀을 저장하는 딕셔너리를 만든다.

```python
teams = {
	"human_resistance" : [bee]
	"destroy_mankind_on_earth" : [sky]
}
```

직접 teams 딕셔너리를 만들었다.

그런데 처음의 person 딕셔너리들과 마찬가지로,
타이핑에 오타가 발생할수도있고, 여러모로 피곤함이 겹친다.

### Q: teams 딕셔너리에 어떻게 자동으로 person을 배치할 수있을까?

### A. person 딕셔너리 생성함수에 teams 자료에 저장하는 로직을 추가하자

```python
team = {}

def create_person_save_team(name, level, team){
     person = {
		"name": name,
		"level": level,
		"team": team
	}
	teams[team].push(person)
	return person
}
bee = create_person("bee", 2500, "human_resistance")
```

하지만, 여기서 끝이 아니다.
분명 더 다양한 요구사항들이 등장할 수있다.

### Q: person에 team 말고도 다른 카테고리 속성도 넣어주자

### A: 연관된 함수를 수정하자,

### Q: person 말고도 terminator 딕셔너리를 추가하자

### A: 딕셔너리 생성함수를 새로 만들고, 카테고리 분류 로직이있는 함수를 전부 업데이트해주자

... 끝이 없다.

처음에 지적한 문제 - 딕셔너리를 언제나 같은 규격으로 재생성하기는
팩토리함수를 통해 해결했다.

하지만 여전히 함수와 특정 자료구조 (teams)가 바인딩 되어있다.

- 다른 속성이나, 다른 분류의 딕셔너리를 추가할때마다 거의 모든 함수를 수정해야함

### 하지만 객체지향을 사용한다면?

데이터와 함수가 묶여있는 구조를 구체적으로 정의하고,
한곳 (class)에 있는 코드의 수정을 통해 모든것에대한 기능을 변경할 수있다

- 즉 , 구체적인 딕셔너리를 만들기위한 같은수준의 추상화를 갖는 무수히 많은 함수들을 반복해서 만들고 수정하는 작업을 간소화 할 수있다.

# 2. 객체지향의 4가지 컨셉

## 2.1 캡슐화: Encaptulation

데이터와 그 데이터에대해 실행되는 함수를 하나의 컨테이너 (class) 등에 담아 놓는것이다.

- 노출할 데이터를 숨길 수있다.
- 객체의 첫 초기화이후로는 특별히 함수를 사용할때, 타겟 데이터를 반복해서 지정해줄 필요가 없다.

### class?

- 구체적인 데이터 값은 다르지만, 같은 기능을 수행할 수있는 객체`Object`를 생성하는 일종의 Factory

- 특정 데이터와 그 데이터에대해 실행되는 함수를 묶어서 구성한다

- 이때, class 내부에 들어있는 함수를 메서드 `method` 라고한다.

```python

class GreatOldOne:
	def __init__(self):
		self.name = "Chloe"
		self.age = 2024
		self.species = "Planet Eater"
		print(self.name, self.age, self.species)

chloe = GreatOldOne()

print(chloe.name, chole.age, chloe.species)
```

python class method 규칙

- class 내부에 있어야함
- 1번째 argument는 self
  - javascript의 this 처럼, 자기참조역할을 한다.

`__init__`: 초기화 (initialize method)
class를 이용하여 instance 생성시, 자동으로 호출되는 함수

## 2.2 추상화 (Abstraction)

구현 세부내용을 숨기고,사용할 수있게 해주는것

- 구현 내용이 바뀌어도, 사용자가 코드를 바꿔줄 필요는 없다.

### 라이브러리 버전 XX.XX

- 소숫점 자리가 변경되었다면, 내코드를바꾸지않아도 된다.

  - 첫 번째 자리: 기능 추가
  - 두 번째 자리: 버그 수정 및 내부 구조 리팩터링

- 정수부분이 바뀌었다면, 내 코드도 변경해야한다!

  - 정수부분은 구현체를 사용하는 코드가 변경되었음을 나타낸다.
  - 최신 버전으로 업데이트 시 내 코드도 수정해야 한다.

## 2.3 상속 Inheritance

- 특정 class가 다른 class를 상속받으면, 상속받은 대상의 데이터나 함수에 자유롭게 접근할 수있다.

- 코드를 재사용하고, 책임을 명확하게 분류해서 프로그램을 작은 단위의 코드로 조직할 수있다.

```python
class Dog:

	def __init__(self, name, kind, age):
		self.name = name
		self.kind = kind
		self.age = age

	def sleep(self):
		print("")

class WildDog(Dog):
	def shout(self):
		print(f"{self.name}!!! ${self.name}!!!")

class Puppy(Dog):
	def __init(self, name, kind):
		super().__init__(name, kind, 0)
	def cry(self):
		print(f"{self.name}!")
```

### Dog의 책임: 기본 정보 속성 제공

WildDog과 Puppy는 둘다 Dog의 속성을 상속받아서,
name, kind, age 속성을 가지고있다.

### WildDog의 책임: 짖을 줄 앎

### Puppy의 책임: 울줄 앎

## 2.4 다형성: Polymorphism

polymorphism의 사전적 의미

```
(prefix)
poly: 여러가지의, 여러 개의

polypeptide(폴리펩타이드): 단백질 -  펩타이드결합이 여러 개 있는것

polyester(폴리에스테르): 석유로 만드는 옷감 -  에스테르결합이 여러 개 있는것

morphism: 형태, 상태변화
morph(모프): 변화하다.
```

### polymorphism in CS

같은 이름, 같은 참조를 사용하는 객체, 함수등이
서로다른 특성이나 행동을 여러가지 보유할 수있는 성질

```
ex)

"hi"+ "hi" === "hihi"
두 문자열을 이은 하나의 문자열을 만들어주는 연산

6 + 7 === 13
두 숫자를 더해주는 연산

위의 예시에서 "+" 연산자는 피연산자의 타입에 따라 전혀 다른 기능을 수행하고 있다.
```

- 대표적예시: method overriding

  - 하나의 이름을 가진 함수가 다양한 형태의 call signature를 가져서 다양한 기능을 수행하는 것

  ***

# Question

### 1. 객체 지향프로그래밍이 무엇인지, 객체는 무엇인지 각각 한줄로 설명해보세요

---

# Answer

### 1. 객체 지향프로그래밍이 무엇인지, 객체는 무엇인지 각각 한줄로 설명해보세요

객체지향프로그래밍: 프로그램을 함수나 로직등이아닌 객체의 집합으로 보는 프로그래밍 패러다임.

객체: 데이터와 그에대한 함수들을 모아놓은 덩어리

---

# References

노마드코더: python-for-beginners #5 OOp

https://nomadcoders.co/python-for-beginners/lobby

https://www.techtarget.com/searchapparchitecture/definition/object-oriented-programming-OOP

https://www.geeksforgeeks.org/introduction-of-object-oriented-programming/
