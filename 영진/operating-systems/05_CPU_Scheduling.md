# CONTENTS

```

 1. 스케쥴링 개요
    - s. 스케쥴링 단계
    - b. 스케쥴링 정책
        - 선점/비선점
    - c. 성능평가 기준

 2. 스케쥴링 알고리즘
    - FCFS
    - SJF
    - SRT
    - RR
    - HRN
    - MLQ
```
---

# DETAILS

## 1. 스케쥴링 개요

### scheduling의 뜻

여러가지 작업의 처리순서를 결정하는 것

### 운영체체에서 스케쥴링의 의미

프로세스를 처리할때, 어떤 순서대로, 얼마 만큼의 시간을 배정하여
cpu를 할당할지 결정 하는 작업을 의미

### a. 스케쥴링 단계


운영채제에는 여러단계의 스케쥴링이 존재

![](https://slideplayer.com/slide/14603556/90/images/7/Principles+of+Operating+Systems+-+CPU+Scheduling.jpg)

|이름|역할|선택기준|
|--|--|--|
|상위단계: <br> Job |시스템에 들어오는 작업을 선택 <br> 프로세스를 생성하여 준비큐에 삽입|I/O 중심 작업과 연산작업을 균형있게 선택 <br> 두 작업별로 많이 필요한 자원이 다르므로, 절충하여 선택해야 자원의 효율적 이용이 가능|
|중간단계: <br> Medium Term| 일시적으로 메모리에서 제거하여 중지시키거나, <br> 재 활성화시켜 단기부하를 조절|어떤 프로세스를 제거해야 시스템 부하가 줄어들지를 기준으로 결정|
|하위단계: <br> CPU|사용가능한 cpu를 준비상태의 프로세스에게 배당 <br> 프로세스를 실행하는 역할|스케쥴링 정책에 따라 다름|



### b. 스케쥴링 정책

1. 선점 스케쥴링: (preemptive scheduling policy)

    - 진행중인 작업에 인터럽트를 걸고 다른작업에 cpu를 할당할 수있는 스케쥴링 전략

    - 성능 최적화를 위한 정책이나, 잦은 문맥교환시 오히려 성능 저하

2. 비선점 스케쥴링: (nonpreemptive scheduling policy)

    - 일단 cpu를 할당받아 실행을 시작한 프로세스는, 프로세스 종료 혹은 프로세스자체의 I/O 요청 전까지 계속 실행하는 스케쥴링 전략

    - 프로세스가 중간에 중지되는일이 적으므로, 실행시간의 예측이 쉬움


### c. 스케쥴링 성능평가 기준

평균 대기시간 (average waiting time) :
- 각 프로세스가 수행이 완료될 때까지 준비큐에서 기다리는 시간합의 평균값

<br>

평균 반환시간 (average Turnaround Time):
- 실행 시간과 대기 시간을 모두 합한 시간으로 작업이 완료될 때 까지 걸린 시간


<br>


## 2. 스케쥴링 알고리즘

### FCFS: First Come First Served

가장 간단한 방식

 준비큐에 도착한 순서에따라 프로세스를 순차적으로 실행하는 비선점 알고리즘

![](https://prepinsta.com/wp-content/uploads/2023/01/FCFS-Scheduling.webp)


### SJF: Shortest Job First


준비큐에서 기다리는 프로세스중 실행 시간이 가장 짧다고 예상되는것부터 실행하는 비선점알고리즘 

![](https://prepinsta.com/wp-content/uploads/2022/06/Non-preemptive.webp)


### SRT: Shortest Remaining Time

준비큐에 대기하는 프로세스중, 남은 실행시간이 가장 짧다고 예상되는것부터 실행하는 선점 알고리즘

대화형 운영체제에 유용

선점 SJF 방식이라고도 불리움

![](https://prepinsta.com/wp-content/uploads/2023/01/Shortest-Job-First-SJF-Preemptive-Algorithm.webp)

### RR: Round Robin 

준비큐에 들어온 순서대로 프로세스를 실행하되,
정해진 할당 시간이 지나면 실행을 중지하고 다시 준비큐의 마지막에 집어넣는 선점 알고리즘

![](https://prepinsta.com/wp-content/uploads/2023/01/Round-Robin-Scheduling-1.webp)

### HRN: (or HRRN): Highest Response Ratio Nest

준비큐에 대기하는 프로세스중, 응답비율(response ratio)가 가장 큰것을 먼저 실행하는 비선점 알고리즘

응답비율은 (대기시간/ 예상 실행시간) + 1 으로 계산한다.

즉, 대기시간이 크고, 예상 실행시간이 짧을 수록 빨리 실행된다.

![](https://s3.ap-south-1.amazonaws.com/s3.studytonight.com/tutorials/uploads/pictures/1607066224-71449.jpg)

###  MLFQ : Multi Level Feedback Queue

준비큐를 여러개 두고, 각각 다른 시간 할당량을 부여하는 비선점 스케쥴링 알고리즘

낮은 단계의 큐에있는 프로세스부터 실행

할당 시간동안 작업을 완료하지 못할시,
 해당 프로세스를 1단계 높은단계의 준비큐로 이동, 그 다음 원래 준비큐의 다음 프로세스를 실행

n레벨의 준비큐에 들어있는 작업은 1부터 n-1레벨까지의 모든 준비큐가 비어있어야지만 실행 될 수 있음

만약 큐가 n개라면, n레벨의 큐, 즉 최 상위레벨 큐에 위치한 작업은 시간 할당이 끝나면, 다시 n레벨의 큐에 삽입
- 최상위 레벨 큐만 생각하면 RR과 같은방식

![](https://s3.studytonight.com/tutorials/uploads/pictures/1606975413-71449.png)
---

# QUESTION

1. os의 단계별 스케쥴링에 대해 설명해주세요.
스케쥴링 대상 및 기준이 포함되어야 합니다.

2. 선점 스케쥴링 방식은 주로 어떤것을 기준으로 먼저 실행할 프로세스를 정하나요?


---

# ANSWER



출처: 운영체제 -김진욱, 이병래, 곽덕훈 공저 / KnowPRESS 출판


라운드로빈: https://prepinsta.com/operating-systems/round-robin-scheduling-algorithm/