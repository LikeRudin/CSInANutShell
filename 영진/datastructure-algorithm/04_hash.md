# Contents

```
    1. Hash 개요
        - 정의
        - 사용이유
        - Hash function/table
        - Hash 충돌

    2. 구현
```

---

# Details

# 1. Hash 개요

## 1.1 정의

### 사전적 정의

```
Hash

감자나 고기를 작게 쪼개서 섞은 요리
기호: #

뭉치다, 으깨다, 섞다, 망치다.

a mixture of meat and potatoes cut into small pieces and baked or fried
the symbol # on a phone or computer keyboard
```

### in Computer Science

```
Hash


key:data mapping 기법의 일종으로,
data에 같은 유형의 key를 부여하여 저장

```

![](https://media.geeksforgeeks.org/wp-content/uploads/20200609180838/HashingDataStructure-min.png)

## 1.2 사용이유

motivation

data 규격을 일치시키면 컴퓨터가 무한한 데이터를 유한한 공간에
효율적인 방식으로 저장할 수있다.

예시

```
 회원 비밀번호 저장

bee님의 비밀번호: flightofheBumblebeein2024

- 20글자 영문 + 숫자

sky님의 비밀번호: skynetiscomingat2024

- 20글자 영문 + 숫자

chloe님의 비밀번호: power!working!never!over!workit!harder!doit!faster!makeit!stronger!

- 62 글자 영문 + 특수문자


▽Hash▽

bee: fff91126765b7a3eb5016061485e0a69

sky: 4bf0005d3147255b34ef22ba76edfeb7

chloe: 80546fd2ac1eb154a9db311d11833a7e


이렇게 다양한 구성, 다양한 길이의 data들을
특정 길이의 소문자 알파벳과 숫자의 조합으로 변환시킴으로써,
저장소의 용량을 쉽게 계산할수 있다.
```

![](https://media.geeksforgeeks.org/wp-content/uploads/20220701080941/ComponentsofHashing-660x342.png)

## 1.2 Hash function/table

### Hash Function

여러 유형의 data를 특정 규격으로 변형시키는 함수

- 멱등성(idempotent)을 갖고있음
  - 같은 입력에는 언제나 출력을 반환
  - 출력을 HashValue라고 부른다.

![](https://cybersecurityglossary.com/wp-content/uploads/2019/07/Hash-function.jpg)

### Hash Table

hashvalue:

Key:value 순서쌍으로
Hash Function에 의해 생성된 값을 저장하는 자료구조
주로 dictionary, array를 사용한다.

## Hash Collision

```
서로 다른 data의 hash value가 일치하여,
저장할 장소에 이미 다른 값이 저장되어있는 현상

무한한 유형의 data를 유한한 개수의 hash value중 하나로 변환시키므로
다른 data지만, 같은 hash value로 변환될 수있음
```

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20220706102035/Collision-in-Hashing.png)

## 충돌회피

### Separate Chaining

Hash table를 단순한 Array가 아니라, LinkedList의 Array로 구현하여,
신규 데이터를 LinkedList의 뒷부분에 새 Node로 추가하여 충돌을 방지함

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2015/07/hashChaining1.png)

- 장점

  - 구현이 쉬움
  - 저장소에 남은 공간이 있는한, 절대 Table이 가득차지 않음.

- 단점
  - 속도가 느림
    - LinkedList에서의 검색은 O(n)의 시간복잡도를 가짐
  - 저장공간 사용 효율이 안좋음
    - HashTable의 다른곳에도 공간이 있어도, 충돌이 일어난곳에 꾸역꾸역 새 Node를 추가함

### Open Addressing

충돌시 원래 저장위치의 다음위치에 신규 데이터를 저장

- 만약 그곳에도 데이터가있다면, 데이터가 없는 슬롯이 나올때까지 이동하여 저장

Seperate chaining과는 다르게, 일반적인 HashTable에 데이터를 저장함.

- Key의 수보다 table의 크기가 더 커야함

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2015/08/openAddressing1.png)

- 장점

  - 자주 저장되는 key를 안다면, 최적화가 쉬움
  - 캐시 성능이 더좋음
    - LinkedList를 타지 않기때문에, 속도가 더빠름

- 단점
  - 구현이 좀더 복잡함
  - Hash table이 가득 차면 별수없음

---

# Question

1. Hash Collision 해결방법중, Open Addressing이 seperate Chaining보다 캐시 성능이 좋은 이유가 무엇인가요?

2. 데이터를 Hash function으로 변환한후에 table에 저장하는것은, 그냥 저장할떄보다 추가적인 연산이 필요한데
   어떤면에서 Hash가 성능을 개선한다는 것인가요?

# Answer

1. Seperate Chaining 방법은, table의 각 슬롯마다 LinkedList를 놓고,
   충돌이 발생하면 새로운 Node를 추가하는 방식으로 작업을합니다. 따라서 데이터를 가져올때
   시간복잡도 O(n)의 탐색이 추가로 요구되지만, Open Addressing은 Array 형태의 HashTable에 모든 데이터를 저장하기때문에
   O(1)의 속도로 검색을 할 수있습니다.

2. 무한한 유형의 데이터를 유한한 규격의 데이터로 변경할수있기때문에,
   데이터 저장공간을 계산하기가 더 쉽습니다. 그래서 저장공간을 더 효율적으로 사용할 수있습니다.

# References

https://www.geeksforgeeks.org/hashing-data-structure/

https://cybersecurityglossary.com/hash-function/

https://byjus.com/gate/hash-function-in-data-structure-notes/

https://github.com/dcodeIO/bcrypt.js/blob/master/src/bcrypt/impl.js#L409

```

```
