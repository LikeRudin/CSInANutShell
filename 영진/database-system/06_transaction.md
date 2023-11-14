# Contents

```
1. Transaction 개요
    - definitions

2. ACID에대한 사실과 오해
    - atomicity
    - consistency
    - Isolation
    - durability

3. 트랜잭션이 초래하는 문제점

4. 트랜잭션의 분류
    - Multi/Single Object transaction
```

---

# Details

# 1. Transaction 개요

## 1.1 definitions

1. 일반 명사 Transaction

   ```
   an exchange or interaction between people.
   an instance of buying or selling something; a business deal.

   사람간 거래나 상호작용
   무언가를 사고 파는 행위. 사업적 거래
   ```

2. Transaction processing

   ```
   Allowing clients to make low-latency reads and writes
   - opposed to batch processing jobs

   client에게 낮은 지연시간의 read 와 write 연산을 제공하는것.
   ```

3. Transaction in Practice

   ![](https://images.contentful.com/po4qc9xpmpuh/3CQA2Vahq9s71Iifwz8SHG/15acd162da3b04a09d5c048aa121ce8d/database-transaction-2.png)

   ```
   a set of related tasks treated as a single action.

   데이터베이스의 상태를 변화시키기 수행하는 작업.
   여러 연산 작업을 하나의 단위로 묶은것.
   ```

# 2. ACID에 대한 사실과 오해

## 2.1 ACID?

- Atomicity(원자성), Consistency(일관성), Isolation(독립성), Durability(영속성)의 약어

- 트랜잭션, 혹은 데이터베이스 연산이 갖춰야 할 특성을 나타내는 용어

- 문제점

  - term Overload: 이미 CS에서 다른 의미로 사용되고있는 용어가 존재함.
  - consistency는 사실 데이터베이스와 관련이없음

    - 어플리케이션의 구현과 관련이 있는 성질

  - 구현의 어려움

    - durability는 실제 구현가능한 성질이 아님

  - 이처럼 ACID를 나타내는 용어들은 상기한것처럼 애매하다는 평가를 받고있음
    - 자신의 DB 제품이 ACID 특성들을 완전히 지원한다고 주장하는 회사는 없음
    - 또 각각의 데이터베이스가 제시하는 ACID에대한 개념이나 형태도 조금씩 다름

### Atomic(원자성)

1. Atomic의 일반적 의미

   ```
   of or forming a single irreducible unit or component in a larger system.

   커다란 시스템속에서 더이상 나눠질수없는 최소단위 혹은 구성요소
   ```

2. in transaction

   ```
   Ensures that either all or none operations of the transactions succeed.
   The transaction is either committed successfully or aborted and rolled back.

   하나의 트랜잭션으로 묶인 연산들은 전부 성공하여 결과가 데이터베이스에 반영되거나,
   전부 실패하여 취소되어야 함
   ```

키포인트

- 트랜잭션 작업 수행중 한 구간에서 오류가 발생하면, 연관된 작업을 모두 폐기 할 수있음

### Consistent(일관성)

1. Consistent의 일반적 의미

   ```
   conformity in the application of something,
   typically that which is necessary for the sake of logic, accuracy, or fairness.

   어플리케이션과 같은것에 존재하는 통일성

   논리안정성, 정확성 또는 공정성을 요구하는 곳에서
   필수적으로 요구되는 성질
   ```

2. in transaction

   ```
   Ensures that the states of the database before and after the transaction are valid (i.e. any existing invariants about the data are maintained).

   트랜잭션 작업 전후의 데이터베이스 상태는 전부 유효한 상태여야 한다.
   ```

키포인트

- 특정 값은 데이터베이스의, 모든 부분에서 같아야한다는 성질

  ```
  ex)
  상업용 프로그램에서 전화번호와 같은 회원정보가
  이런저런 테이블에 동시에 존재한다면,
  한 회원에대한 정보는 어떤 테이블에서도 같아야함
  ```

### Isolated(독립성)

1. Isolated의 일반적 의미

   ```
   far away from other places, buildings, or people; remote.

   다른것들로부터 떨어져있는, 고립되어있는
   ```

2. in transaction

   ```
   Ensures that concurrently running transactions have the same effect
    as if they were running in serial.

   병행처리되는 트랜잭션들을 순차적으로 실행하여도
   똑같은 효과를 가져야 한다
   ```

- 키 포인트: 여러개의 트랜잭션들이 동시에 실행되어도,
  각각의 트랜잭션의 서로의 결과에 영향을 미치면 안됨

### Durability(영속성)

1. Durability의 일반적 의미

   ```
   the ability to withstand wear, pressure, or damage.

   여러 해악으로부터 손상을 입지않는 능력
   ```

2. in transaction

   ```
   Ensures that after the transaction succeeded,
   any writes are being stored persistently.

   트랜잭션이 성공하고나서, 그 결과는 데이터베이스에 영구적으로 반영되어야함
   ```

키포인트: 연산의 결과는 정상적으로 데이터베이스에 완전히 반영되어야하며,
훼손되면 안됨

## 2.2 ACID의 문제점

용어의 사용에 대한 여러 불협화음이 존재

1. 트랜잭션

- 클라이언트에게 저 지연 읽기/쓰기를 제공하는 연산
  - ACID 성질을 갖는 연산단위가 아님.

2. ACID

- 용어의 중복사용 및 애매함이 존재
- 실제 트랜잭션을 대표하는 성질이 아님

| 이름        | 문제점                                    | 설명                                                                                                                                                                                                                                                                                                                                          |
| ----------- | ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Atomic      | 동음 이의어의 CS용어가 존재               | ex) 트랜잭션의 atomic은 작업을 한단위로 묶는것을 강조함. 오류 발생시 전체 취소 가능한 성질을 의미 <br> 반면 멀티스레딩에서 Atomic은 스레드끼리 서로의 작업에 침범할 수없음을 의미함 <br> 또, 데이터베이스에서도 Atomic Operation이라고하면 Read-modify-write의 과정을 거칠 필요없이 한번의 쿼리로 값을 수정하는 increment와 같은연산을 의미함 |
| Consistency | 데이터 베이스에서 제공하는 성질이 아님    | 연결된 데이터끼리 자동 업데이트되도록 관계를 설정해주는것은 어플리케이션의 문제, <br> 개발자가 구현할 문제이지 DB가 스펙으로 제공하는것이 아님                                                                                                                                                                                                |
| Isolated    | 트랜잭션의 대표속성으로서의 자질이 부족함 | 일반적으로 데이터베이스들이 중요하게 고려하지않음, <br> 심지어 Oracle은 아예 지원도 안함                                                                                                                                                                                                                                                      |
| Durability  | 구현이 불가능함                           | 영속성은 지향점이지, 갖출수있는 성질이 아님. <br> 하드와 서버가 다박살나면 어떻게 할건데?                                                                                                                                                                                                                                                     |

# 3. 트랜잭션에 의한 문제점

오히려 결과의 성공을 한번에 반영한다는점이 문제가 될수도있음

rollback의 단점

1.  중간 작업결과의 손실은 치명적

    - 전체 작업은 성공했으나 네트워크 오류가 발생하여 한가지 연산의 결과 저장에 실패한경우
      모든과정을 다시수행해야 함

    - 컴퓨팅자원의 낭비가 극심

2.  트랜잭션에 안주하게되면 더 나은 오류제어 방법을 고안하는데에 게을러짐

# 4. 트랜잭션의 분류

## Multi Object Transaction

데이터 동기화를위해 여러개의 자료구조 (row, table,record )등을 한번에 업데이트해야하는 연산

- 일반적으로 트랜잭션을 일컫을때 이 연산을 나타냄

- 현재 noSQL및 여러 Db관련 프로그램에서 사용을 줄이고 있음
  - 분산저장 DB시스템의 사용 또는 db scale 변경작업에서 치명적으로 불편함
  - prisma orm이 지원하는 Transaction api도 단 하나의 네트워크요청만 발생시킴

## Single Object Transaction

하나의 자료구조만 한번에 업데이트 하는 트랜잭션

# Question

### Q1.

트랜잭션의 성공은 무엇이고, 어떤 조건이 필요한가요?

### Q2.

분산형데이터베이스에서 트랜잭션을 사용하는것은 어떤 불편함을 초래하나요?

# Answer

###

### A1.

작업결과가 온전히 DB에 반영된다면 트랜잭션에 성공했다고 합니다.

성공을 위한 조건은 트랜잭션을 구성하는 작업들이 전부 오류없이 수행되는것입니다.

### A2.

트랜잭션으로 묶은 연산의 수행대상이 하나의 데이터베이스에 전부 저장되어있는것이아니라면,
각 데이터에대한 작업을 묶어주기가 굉장히 난해해지고, 네트워크 상태 및 db별 응답 지연속도에따라
트랜잭션의 실패율이 올라갑니다.

# References

https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Database/Transaction.md

https://www.techtarget.com/searchcio/definition/transaction
https://fauna.com/blog/database-transaction
