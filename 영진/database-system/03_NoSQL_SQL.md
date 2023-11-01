NoSQL VS SQL

# 1. No SQL이란?

Not only SQL이라는 뜻으로, SQL 공식 표준안을 따르지 않는 모든 데이터베이스를 뜻함

# 2. SQL이 시장을 지배할 수있었던 이유

현재는 SQL이 아닌 DB를 NoSQL이라고 부르는만큼 SQL의 지위는 시장에서 확고하나
과거에는 무수히 많은 선택지중 하나였습니다.

당시DB는 `hierarchical` vs `relational` vs `network` 이렇게 세가지 구조가 있었습니다.

![](https://t2.daumcdn.net/thumb/R720x0/?fname=http://t1.daumcdn.net/brunch/service/user/oZ3/image/fselOiYGteDJhMLUgDSwEGAw7Pg)

 <div class="image-container" style="display:flex">
        <img src="https://media.geeksforgeeks.org/wp-content/uploads/20200727111632/hierarchical.png" style="width:30%" alt="Hierarchical">
        <img src="https://media.geeksforgeeks.org/wp-content/uploads/20200729160044/relational.png"  style="width:30%" alt="Relational">
        <img src="https://media.geeksforgeeks.org/wp-content/uploads/20200727113000/network.png"  style="width:30%" alt="Network">
</div>

하지만 SQL은 경쟁자에비해 `트랜잭션` 과 `JOIN`기능이 매우 강력했고,
이로인해 시장을 순식간에 점거하게 됩니다.

## 2.1 트랜잭션

의미

- Transaction: 거래나 교환을 포함한 상호작용

## 2.2 DB의 트랜잭션

- 특정 작업을 묶어서 한 단위로 처리하게 하는 기능

은행거래나 상품 구매등의로직을 구현하는데에 사용

![](https://t1.daumcdn.net/cfile/tistory/9978DA4F5ADE84AD15)

### SQL은 왜 트랜잭션에 강했나?

1. 표준의 존재

SQL은 트랜잭션 관리에 대한 명확하게 정의된 널리 사용되는 표준을 가지고 있었습니다.

2. 트랜잭션 구현이 쉬움

SQL의 경쟁자들은 데이터가 저장된 경로를 외워야 쿼리를 수행할 수있었습니다.

마치 현재의 우리가 JS객체내부의 특정 프로퍼티에 접근하려면
null체크를 하면서 길을 하나하나 집어가듯이, 말입니다.

```js
myObject?.movieList?.
```

그래서 특정 쿼리를 요청하거나 데이터를 조작하는것도 쉽지가 않았는데,
여러개의 쿼리를 묶어서 요청하고 취소하게 하는로직을 만드는것은 상당히 고통스러운 작업이었습니다.

## 2.2 DB의 JOIN

JOIN은 여러 데이터를 병합하는 기능입니다.

데이터 저장 구조때문에 Mysql은 경쟁자들보다 훨씬 간단하면서도 강력한 JOIN을 제공하였습니다.

계층 구조나 네트워크 구조는 표준화된 테이블 구조를 가지고있지 않았기때문에,
데이터를 합치고 정렬하는것 자체가 굉장히 어려운 작업이었기 때문입니다.

우리가 서로다른 두 API에서 받은 데이터객체를 잘 정렬해서 합쳐서 하나의
객체로 보기좋게 나타내는것을 생각해보면 쉽게 이해할 수 있습니다.

### NoSQL

2009년 MySql을 오라클이 인수하면서, MYSqL의 유료화가 진행되었습니다.

완전한 유료화는 아니었습니다. 비영리 /영리 사이트 서버에는 금액이 징수되지않았고
DB를 튜닝해서 그걸로 장사를 할때에만 금액을 지불하게되었죠.

하지만 언제 오라클의 정책이 바뀔지 모르는일이고, 한번 유료화된 MySQL에 의지하다가
나중에 얼마나 많은 대가 (금전 or 마이그레이션)을 치루게될지는 모르는일이었습니다.

특히 다음 세가지의 요구사항에 앞서 NoSQL이 숨을쉬는 시대가 되었습니다.

- 무료 오픈소스 DB
- 개인화된 데이터구조
- 한 개체의 관계 값들에대한 빠른 쿼리
  - Nested One to Many

### 효율 추구

과거와다르게 SNS가 발달하였고, 요구되는 쿼리는 점차 다양해졌습니다.
특히 SNS의 페이지에서 필요한 데이터구조는 SQL과는 판이하게 달랐습니다.

![](https://camo.githubusercontent.com/853b6301ee21db5db2acf976a4be7b1f59c8e79d1f82bf4f5662ce1a9c90ba57/68747470733a2f2f73332d65752d776573742d312e616d617a6f6e6177732e636f6d2f6a752e7075626c69632f676974732e6769746875622e636f6d2f4d617274696e2532304b6c6570706d616e6e2532302d25323044657369676e696e67253230446174612d496e74656e736976652532304170706c69636174696f6e732f466967757265253230322d312e706e67)

객체지향을 따르는 대부분의 프로그래밍 언어와 다르게 MYSQL은 Relational 패러다임을 가지고있었고, DB언어를 잘 모르는 개발자가 SQL을 사용하기에 어려운것은 ORM으로 해소할 수있습니다.

하지만 쿼리 자체가 비효율적입니다.

그냥 JSON형식으로 데이터를 관리한다면 그냥 하나의 데이터 객체를 로드하면 되는데,
잘 정규화된 DB구조에서는 여러 관계에관한 병합연산을 수행해야하기때문이죠.

### PolyGlot 추구

또 무수한 기술이 떴다 지는것을 반복하는걸 지켜본 시장은,
하나의 기술에 안주하면 큰일난다는 생각을 갖게되었습니다.

SQL 표준을 넘어선 다양한 기술스택을 보유하기위해 노력하게되었죠.

### 결론

Only SQL의 시대는 가고, 다시 다른 DB기술도 사용하는 시대가 되었지만,
여전히 SQL이 메인이고 제일중요합니다.

# Reference

# QUESTIONS

1. Nested One to Many 구조의 데이터를 SQL에 관계형 데이터로 저장했을때와 MongoDB에 JSON 형태로 저장했을때 어떤 차이점이 있나요? 쿼리할때는 어떤것의 성능이 더좋나요?

2. 관계형 데이터가 JSON에비해 Many-to-One관계의 데이터를 쿼리하기 수월한 이유가 무엇인가요?

# Answer

1. sql vs mongoDB: Nested One to Many 구조의 데이터를 저장하고 쿼리할때의 차이점
   SQL의 정규화 패턴을 잘 따른 관계형 구조로 데이터를 저장시,
   데이터를 중복없이 저장할 수있습니다.

하지만 해당 데이터에 접근할때 복잡한 JOIN연산및 서브쿼리를 쿼리를 작성해야합니다.

반면 MongoDB에 데이터를 JSON형태로 저장한다면 JOin등의 병합연산이 없는 간단한 쿼리로 전체 데이터를 불러올수있고, 내부의 필드들에 바로 직접 접근할 수 있습니다. 하지만 데이터가 중복될 가능성이 커집니다.

2. 관계형 모델이 JSON보다 Many to one에서 강력한 이유

SQL의 정규화 패턴을 잘 따른 관계형 구조로 데이터를 저장시,
데이터를 중복없이 저장할 수있습니다.

하지만 해당 데이터에 접근할때 복잡한 JOIN연산및 서브쿼리를 쿼리를 작성해야합니다.

반면 MongoDB에 데이터를 JSON형태로 저장한다면 간결한 쿼리로 전체 데이터를 불러와 내부에 있는 데이터들에게 직접 접근할 수 있습니다. 하지만 데이터의 중복성될 가능성이 커집니다.

잘 정규화된 관계형 데이터베이스에서는 분리된 각 모델별 데이터에 직접 접근이 가능합니다. 그러므로 관계를 쉽게 추출하거나 관리할 수있습니다.

JSON은 관계형 데이터 모델의 정규화처럼 잘 표준화된 어떤 기준이 없으므로 Many to one 관계를 다루기가 어렵습니다.

물론 개발팀이 외래키나 필드이름등으로 관계 관리에대한 표준을 만들어서 Many-to-One에 대처할 수있습니다.

https://jerryjerryjerry.tistory.com/48

Data Intensive Application
