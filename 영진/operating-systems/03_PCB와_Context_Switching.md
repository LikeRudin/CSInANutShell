# CONTENTS

```
- PCB
    - 개념
    - 구성요소

- Context Switching
    - 문맥
    - 문맥교환
```
---

# DETAILS

## PCB: Process Control Block

### 개념

프로세스의 관리를 위해 운영체제가 프로세스 정보를 보관하는 블록
- 프로세스의 진행에따라 내용이 변경됨
- 프로세스 종료시 제어블록도 삭제

### 구성요소



![](https://prepinsta.com/wp-content/uploads/2023/01/Process-Control-block-in-os.webp)

운영체제마다 조금씩 차이가 있을 수있으나
대략적으로 다음과 같은 정보들을 저장

|명칭|기능|
|--|--|
|포인터 <br/>Pointer |부모 프로세스를 가리키는 포인터|
|프로세스 상태 <br/> Process Stage| 준비,실행 등 프로세스의 현재 상태|
|프로세스 아이디 <br> Process ID or PID |프로세스의 구분기준이되는 고유한 정수값|
|프로세스의 권한 <br/> Process Privileges |특정 자원이나 장치에 접근가능여부|
|프로그램 카운터 <br/> Program Counter |프로세스 수행을 위한 다음 명령어의 주소|
| 우선순위 <br/> Scheduling Information| 스케쥴링 알고리즘에 따른 우선순위 정보|
|메모리 관리정보 <br/> Memory Management Information |메모리 제한선, page 테이블, segment테이블등 메모리 관리를 위한 정보|
|레지스터 정보 <br/> CPU Registers|프로그램 실행 전에 CPU가 정보를 저장한 레즈스터의 주소들|
|회계정보 <br> Accounting Information|프로세스 실행 시간, 메모리사용량, 기타 시스템 프로그램 사용실태 등 성능 측정과 스케쥴링 순위 배정을 위한 정보|
|입출력 정보 <br> I/O Status| 프로세스가 사용할 수있는 입출력 장치 정보|

## Context switching: 문맥교환

### Context: 문맥?

CPU의 레지스터와 프로세스의 상태

### Context switching: 문맥교환?

CPU가 인터럽트를 요청받아 실행중인 프로세스를 멈추게 하고
다른 프로세스를 수행하게 하기위한 문맥 관리 작업

실행중이던 프로세스의 제어블록에 정보를 저장하고
요청받은 프로세스의 제어블록에서 작업 정보를 불러오는 작업을의미


![](https://prepinsta.com/wp-content/uploads/2023/01/Context-Switching-in-OS.webp)


---

# QUESTION

프로세스 제어블록은 어떤 내용을 담고있나요?

---

# Answer


출처 : https://prepinsta.com/operating-systems/process-control-block/
