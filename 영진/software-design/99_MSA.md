# Contents

```
    1. 마이크로 서비스 개요
        - 모노리스 구조
        - 마이크로서비스 구조
        - 모노리스 vs 마이크로 서비스

    2. 마이크로 서비스 아키텍쳐 패턴
        - 팀 구성
            - 교차기능 팀
        - 프론트엔드
            - 클라이언트 서버구성
                - 모노리스 vs 마이크로
        - 통신방식
            - 동기
            - 비동기
        - 백엔드
            - 저장소 분리 패턴
            - 분산 트랜잭션: SAGA
            - CRQS
            - 이벤트 소싱
        - 아키텍쳐
            - 인프라 구성요소
            - 플랫폼





```

---

# Details

# 1. 마이크로 서비스 개요

## 1.1 모노리스 구조

> `M`onolithic `A`rchitecture

![](https://cdn.ttgtmedia.com/rms/onlineImages/microservices-monolithic_architecture-f.png)

- 하나의 단위로 개발되는 애플리케이션에대한 구조
- 일반적으로 `사용자 인터페이스 / 서버 / 데이터 베이스` 와 같은 3tier 구성을 따름

- 전체서비스의 모든 구성 성분이 논리적인 단일체

  - 애플리케이션을 업데이트 할때에는 전체를 새로 빌드해서 배포.
  - 단일 프로세스로 실행.

- 빌드, 테스트, 베포 모두 쉬운편

- 횡단 관심사 분리 `cross-cutting concerns` 역시 쉬움
  - 클라이언트로의 요청 처리를 하는 서버가 하나이기 때문
  - 응답 서버가 하나이기때문에 로그인 유지, 유저인증, 세션관리에 대한 걱정이 크게 필요없음

**tier** ?

```
- 애플리케이션 구조의 물리적 분리 단계를 나타내는 용어
    - layer는 논리적 분리단계를 뜻함
```

## 1.2 마이크로 서비스 구조

> `M`icro `S`ervice `A`rchitecture

![](https://cdn.ttgtmedia.com/rms/onlineImages/microservices-microservices_architecture-f.png)

- 여러개의 서비스들의 인스턴스가 뭉쳐서 하나의 전체 서비스를 실행하는 구조

- 전체 서비스는 독립적인 서비스들의 집합체

  - 애플리케이션을 업데이트할 떄에는 업데이트 하는 모듈부분만 새로 빌드해서 배포
  - 각 서비스가 독립적이므로 서로다른 기술 (언어 , 프레임워크)를 가지고있음
  - polyglot하게 운영할 수있음

- 처음 구축할때, 빌드 및 배포 과정이 피곤한편
  - 전체 서비스를 구성하는 각 부분을 전부 하나의 서비스로 베포해야 하기 떄문

**_polyglot_**?

```
특정 서비스를 구축할때, 사용하는 언어나 저장소를 자율적으로 선택할 수있는 방식

MySQL이 유료화를 선언한후, NoSQL이 득세한 이유가 바로
다양한 기술을 써서, polyglot하게 서비스를 구축하기 위해서임

- 특정 기술에 종속되면 얘기치못한 비용을 치룰수있기때문
```

![](https://www.researchgate.net/profile/Elhadj-Benkhelifa/publication/337634352/figure/fig5/AS:960270583214080@1605957756008/Microservices-software-architecture-with-polyglot-programming.png)

## 1.3 모노리스 vs 마이크로 서비스

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*Jx5sDRski-xKIwlH.png)

|                      | 모노리스                                                                                                                                                                    | 마이크로 서비스                                                                                                                                        |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 구축 난이도          | 쉬움 ⬇                                                                                                                                                                      | 어려움 ⬆                                                                                                                                               |
| 성능                 | 상대적으로 빠름 ⬆ <br> 하나의 단일 구조이므로, 내부 통신이 거의없음                                                                                                         | 상대적으로 느림 ⬇ <br> 각 서비스끼리 통신이 필요함                                                                                                     |
| 유지보수 <br> 유연성 | 낮음 ⬇<br> 아주 작은 업데이트가 있어도 전체 애플리케이션을 재 배포해야함 <br> 특정 기능에 트래픽이 몰려도, 전체 규모를 키워야함 <br> 다양한 기술을 사용하는데에 한계가 있음 | 높음 ⬆<br> 업데이트를 할때에, 필요한 모듈만 업데이트 해주면 됨 <br> 트래픽이 몰리는곳만 서버를 증설할 수있음 <br> polyglot하게 서비스를 구축 할 수있음 |

---

# 2. 마이크로 서비스 아키텍쳐를 위한 기술

## 2.1 팀 구성

### 교차기능 팀

> cross-functional-team

![](https://www.altexsoft.com/media/2023/03/word-image-7.png)

- 다른 팀과의 협업 없이 독자적으로 운영되는 팀
- 프론트개발자, 서버개발자 / DBA / Devops / Data 분석가 등 전부 한 팀에 있음

> 전통적인 팀구조와의 비교

|           | 전통적 팀                                                                                                                        | 교차기능팀                                                                                                                |
| --------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| 팀 구성원 | 한 분야의 전문가들로 구성                                                                                                        | 서비스 개발 및 운영에 필요한 모든 인력                                                                                    |
| 업무      | 어플리케이션 서비스 계층의 `한 부분` <br> 프론트 개발 / 백엔드 개발 / 배포 및 운영 / 디자이너 / PM 등의 업무중 `한가지`만 수행함 | 어플리케이션에서 특정 서비스의 `전체 부분` <br> 팀 하나가 프론트 / 백엔드 개발 및 운영 베포 프로덕트 관리등 `전부` 수행함 |
| 장점      | 전문지식의 깊이가 깊어서 `각 분야에서 최고의 작업`을 수행할 수있음                                                               | 협업을 위한 `소통이 굉장히 민첩`하고 빠르게 이루어질수있음                                                                |

## 2.2 프론트엔드 패턴

### 2.2.1 클라이언트 서버구성

> 모노리스 구조 vs 마이크로 프론트엔드 구조

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677241355113/27544ad2-9d7c-4a5c-819c-84ac6314a457.jpeg?auto=compress,format&format=webp)

- 모노리스 구조

  - 프론트엔드 서버를 하나로 구성하는것
  - 개별적 서비스에게 API요청을 전부 보냄

- 마이크로 프론트엔드 구조 - UI에 연결된 서버별로 독립적인 프론트엔드 서버를 갖는것 - Composite UI와 혼용해서 사용됨
  ![](https://www.oreilly.com/api/v2/epubs/9781788390415/files/assets/73dda3ad-a484-4091-910a-a683ad891fa5.png)

## 2.3 통신방식

### 2.3.1 동기 통신

![](https://cdn.ttgtmedia.com/rms/onlineimages/synchronous_api_call-f.png)

- 전통적이고 기본적인 통신방식
- 통신 결과가 주어질때까지 다음일을 하지 않음

- 클라이언트에서 서버측에 존재하는 REST API를 호출할 떄에 이용하는 패턴

**Rest API호출이 동기방식** ?

```
프로그래밍 언어를 사용할때에는, Requset를 보내고, Response를 받을때까지
다른 코드가 진행된다.

즉, 코드가 순서에 맞춰서 진행되지않으므로, 이를 비동기 코드라고 부른다.
- 순서대로 코드를 실행하지 않음

하지만 통신에대해 이야기 할때에는,동기방식이라 한다.

Request요청 후 Response를 받기 전까지 계속 기다리면서 재호출을 하기 때문이다.
- 순서대로 통신을 수행함
```

<br>

### 2.3.2 비동기 통신

![Image 1](https://i0.wp.com/www.dineshonjava.com/wp-content/uploads/2019/05/asynchronous-one-to-one.png?w=607&ssl=1)

![Image 2](https://i0.wp.com/www.dineshonjava.com/wp-content/uploads/2019/05/asynchronous-one-to-many.png?w=586&ssl=1)

- 메시지 브로커에 요청을 전달하고, 다음일을 처리
- 통신결과랑 관계없이 그냥 일을 계속 함
- 통신과정의 완결성이 보장되지 않음

- 클라우드 서비스 환경에서, 특정 서비스가 다운되었을때 대응하거나 서비스를 쉽게 확장하기 위해 이용하는 패턴

- 메시지 브로커로는 Apache Kafka, RabbitMQ, ActiveMQ 등의 기술을 이용함

**동기식 vs 비동기식 예시**

```
동기식 취준

한 회사에 이력서를 넣으면, 해당 회사의 확실한 불합격 발표가 있기전까지는
다른곳에 지원하지 않음.

채용 프로세스의 완결성이 보장됨

서류탈락이든, 과제탈락이든,합격이든, 채용프로세스가 온전히 마무리 되어야 다른 회사에 이력서를 넣음


비동기식 취준

특정 회사들이 콜을 하던 말던 계속 이력서를 넣음.
과제를 받으면 과제를 하면서도 계속 이력서를 넣음

채용 프로세스의 완결성이 보장되지않음

A회사의 과제를 하는데, 더 마음에 드는 B 회사의 과제도 받으면
B의 과제를 하다가 붙을 A도 놓치고  B도 떨어질 수있음
```

## 2.4 백엔드 패턴

### 2.4.1 저장소 분리 패턴

전체 서비스를 구성하는 기능별 마이크로 서비스마다 분리된 DB를 제공하는 패턴

**전통적인 애플리케이션의 Monolith DB구조**

![](https://www.c-sharpcorner.com/article/introduction-to-saga-pattern/Images/Monolith%20design.jpg)

```
youtube clone 강의코드를 보면 userRouter, videoRouter등으로
컨트롤러를 모아놓는 모듈을 분리했지만,

결국 각 controller에서 접근하는 database는 mongodb로 운영되는
DBMS하나뿐
```

- 이러한 구조로 DB를 사용한다면 데이터 중심 어플리케이션이라 부름

> 저장소 분리 패턴

각 마이크로 서비스별로, DB를 분리하는 패턴

![](https://www.researchgate.net/profile/Elhadj-Benkhelifa/publication/337634352/figure/fig5/AS:960270583214080@1605957756008/Microservices-software-architecture-with-polyglot-programming.png)

각 마이크로 서비스는 다른 서비스의 api를 통해 해당 도메인의 데이터에 접근함

```
비디오 서비스가 회원서비스의 데이터를 필요로 하면,
회원 서비스의 db에 직접 접근하지 못하고, 회원 서비스가 제공하는 api로 데이터 요청을 보냄
```

### 2.4.2 분산 트랜잭션 처리: Saga 패턴

![](https://microservices.io/i/sagas/From_2PC_To_Saga.png)

- 여러개의 마이크로 서비스가 묶여있는 하나의 트랜잭션을, 내부에서 마이크로 서비스 단위로 분리하는 패턴

- 트랜잭션을 각 서비스단위별로 분해한 `로컬 트랜잭션`과 해당 트랜잭션들을 롤백하는 `보상 트랜잭션`을 이용

### 2.4.3 CQRS 패턴

> CQRS: Command and Query Responsibility Segregation

읽기와 쓰기 용도 대상을 분리하는 패턴

- CRUD를 하나의 대상에 대해 수행하지않고, CUD와 R 을 분리하는것
  - Query와 Mutation이라고 하기도 함

사용이유

```
비즈니스로직이 추가됨에따라 update를 수행하기 위해서 model이 점점 복잡해지는 경향이 있음
그러면 model과 함께 update로직과 더불어 read로직도 함께 복잡해짐

또, 하나의 대상에 많은 update와 read가 동시에 수행될경우, 경합이 발생되어 데이터의 정합성이 보장되기 힘듦

따라서, 읽기 책임(read) 를 갖는 대상과 쓰기 책임(create, update, delete)을 갖는 대상을 분리하여
트래픽을 줄이는 방법으로 리소스 교착상태의 발생 가능성을 줄이기 위해 사용됨
```

**_Model 분리_**

![](https://learn.microsoft.com/ko-kr/azure/architecture/patterns/_images/command-and-query-responsibility-segregation-cqrs-basic.png)

**_DB 분리_**
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*TaPzEj91HM06UgZoajqGwA.png)

### 2.4.3 이벤트 소싱 패턴

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*SQikYTvGayDrXBd-v4RVSg.png)

- 특정 시점에서의 DB의 스냅샷을 만들어놓고, 트랜잭션을 저장하는 이벤트 저장소를 두어, 스냅샷과 이벤트 저장소의 데이터를 통해 쿼리 결과값을 제공하는 패턴

- 데이터 불일치 `정합성`문제를 위해 `write`작업을 최적화 하는 패턴

> 이벤트 소싱 패턴과 전통적인 패턴의 비교:: 커피 매출 / 재관리 시스템

전통적인 방식

```
- 주문 시작

order 객체 생성: 주문된 커피 정보를 기록
money 객체 생성: 매출 정보를 기록
ingredient 객체 생성: 사용된 재료와 재고 감소 정보를 기록

- 결제 및 판매 완료

ingredient 정보 업데이트: DB의 재료 재고 정보를 변경
money 정보 업데이트: DB에서 매출액 및 현재 보유 금액 등의 금액 관련 내용을 업데이트
order 정보 처리: 진행 중인 주문을 마무리하고, 판매 관련 정보를 관리하는 또다른 order 객체를 생성, 정보 기록

-- DB 업데이트가 빈번하게 발생함
    -- 성능 저하 유발
```

이벤트 소싱 패턴을 사용하는 경우

```
- 특정 시점의 데이터베이스 상태를 나타내는 스냅샷 제작

- 모든 트랜잭션(주문 시작, 결제, 재료 사용 등)를 이벤트 저장소에 기록
    - 이벤트 시간 순서대로 저장
    - 변경이 불가능한 불변 객체로 저장

- 영업 시간

새로운 주문을 처리할 때, 스냅샷과 이후의 이벤트들을 함께 계산하여 데이터의 최신 상태를 파악

- 영업 종료후

커피집이 문을 닫은 밤에 이벤트 저장소의 트랜잭션을 실제 DB에 반영
-- 시스템 부하를 최소화하고 안정적인 성능을 유지

```

## 2.4 아키텍쳐 패턴

### 2.4.1 인프라 구성요소

> 인프라

IT환경을 운영하고 관리하는데 필요한 근간이되는 하드웨어, 소프트웨어, 네트워킹 구성요소, 운영체제 등등

> 서버 구성

**Bare Metal Server Vs Iaas**

- Bare Metal Server

  - cpu, 메모리등의 컴퓨팅 자원을 구입해서 물리적으로 직접 서버를 구축

- Iaas: InfraStructre as aservice
  - 시스템 자원구성, 할당,관리, 모니터링등을 클라우드 서비스로 제공받음

**Iaas를 주로 사용한다**

마이크로서비스는 주로 엔터프라이즈급 서비스구성에 이용

- 서비스는 어떠한 환경에서도 실행되어야함
- 당연히 가상인프라 환경을 선호함

> 환경구성 (가상 인프라)

**Virtual Machine vs Container**

- Virtual Machine

  - Hypervisor라는 소프트웨어를 이용, 하나의 시스템에서 여러 운영체제를 사용
  - 게스트 OS를 보유
    - 운영체제 패치와 같은 오버헤드가 지속적으로 발생

- Container
  - container 프로그램의 엔진을 통해 가상 격리환경 생성

![](https://www.netapp.com/media/container-vs-vm-inline1_tcm19-82163.png?v=85344?v=85344)

마이크로 서비스를 사용할때에는 아주 작은 서비스를 독립적으로 배포함

**Container를 주로 사용한다**

- OS없이 격리환경을 제공하는 container가 선호
- 인기있는 제품으로는 Docker가 있음

**Container Orchestration**

다수의 container를 관리하기위한 기술

- container관리동안 발생하는 여러 문제를 해결

  - container자동 배치 및 복제, 장애 복구, 확장 및 축소, 통신, 로드밸런싱..

- Docker Swarm, Apache Mesos등의 제품이 있다.
- 인기있는 것으로는 구글이공개한 자사솔루션, Kubernetes가 있음

### 2.4.2 플랫폼 패턴

> 플랫폼

인프라위에서 애플리케이션을 개발, 실행, 관리하는 환경을 의미

- 개발`Dev`
- 운영`Ops`,
- 지속적통합`C`ontinuous`I`ntegration,
- 지속적 배포 `C`ontinuous `D`eploy

등등에 이용되는것을 말함

> Paas: Platform as a Service

Iaas와 마찬가지로 Platform을 서비스로 제공받는것

# Question

1. 마이크로 서비스 아키텍쳐를 구성하는 가장 중요한 이유가무엇인가요?

2. CRQS의 정의에 대해 설명하고, 이것이 필요한 이유를 말씀해주세요.

3. 동기 통신방식과 비동기 통신방식에 대해 설명해주시고, 각 예시를 하나씩 제시해주세요.

---

# Answer

1. 마이크로 서비스 아키텍쳐를 구성하는 가장 중요한 이유가무엇인가요?

2. CRQS의 정의에 대해 설명하고, 이것이 필요한 이유를 말씀해주세요.

3. 동기 통신방식과 비동기 통신방식에 대해 설명해주시고, 각 예시를 하나씩 제시해주세요.

---

# References

https://www.techtarget.com/searchapparchitecture/tip/Pros-and-cons-of-monolithic-vs-microservices-architecture

https://www.oreilly.com/library/view/spring-50-projects/9781788390415/4a2a10c0-60a5-40e4-a5b0-7cf3e25d0f22.xhtml

https://www.techtarget.com/whatis/definition/synchronous-asynchronous-API

https://naderfares.medium.com/12-essential-patterns-for-microservices-architecture-920cc62eb63d

https://www.dineshonjava.com/microservices-inter-service-communication/

https://microservices.io/patterns/data/saga.html

Data intensive Application

- Martin Kleppmann
- Oreilly

도메인 주도 설계로 시작하는 마이크로 서비스 개발

- 한정현, 유해식, 최은정, 이은정
- 위키북스
