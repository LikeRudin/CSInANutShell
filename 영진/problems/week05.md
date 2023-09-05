클로이
```
1. 로드 밸런싱이 필요한 이유에 대해서 설명하시오
👀
2. blocking과 synchronous는 동일한 것인가? 그렇지 않다면 그 이유에 대해서 설명하라.
3. blocking / sync로 인한 4가지 상황에 대한 실제 사례를 찾아보시오.
```

bee
```
👀
1. L4 Load Balancing과 L7 Load Balancing의 차이
2. 서버 이중화를 하는 이유
3. Non-blocking I/O의 프로세스는?
```

choi
```
1. Load Balancing은 왜 사용하는가?
👀
2. VIP(Virtual IP)는 무엇인가?
3. Blocking/Non-Blocking과 Async/Sync의 차이는 무엇인가?
```

sky

```
Q1. 로드밸런싱에서 확장성, 성능, 보안에 대한 장점이 있는데 이에 대해서 그 이유를 서술하시오
Q2. Async Non-block 시스템에서 Block이 생기는 경우는 무엇이 있을까요
Q3. Blocking I/O 가 리소스를 낭비하는 이유는 무엇인가요
👀
Q4. OSI 7계층에서 소프트웨어가 담당하는 응용-표현-세션-전송-네트워크 역할을 간단히 서술하시오
```

--- 

# ANSWER

## sky님의 질문

 OSI 7계층에서 소프트웨어가 담당하는 응용-표현-세션-전송-네트워크 역할을 간단히 서술하시오

### 응용 계층 (Application Layer):

소프트웨어와 사용자간 직접 상호작용이 이루어지는 곳
브라우저나 이메일 같은것이 포함

- 사용자에게 유저 인터페이스를 제공 
- 입력받은 데이터 전송 및 수신

### 표현 계층 (Presentation Layer)

말그대로 표현을 담당하는 곳
여러 데이터 파일 형식에대한 작업을 수행
- 이미지 파일이나 소리파일, 텍스트파일별로 다양한 확장자가 있음
- 암호화 및 복호화도 수행

### 세션 계층 (Session Layer):


- 데이터 교환 세션을 괸리하는곳
- 흐름제어, 타이밍 동기화 등등을 함

```
세션은 특정 작업을 위한 기간을 의미
`a period devoted to a particular activity.`
```

### 전송 계층 (Transport Layer):

데이터 전송과 관련된 일을 수행하는 곳

전송을 위해 데이터를 작은 단위로 쪼개거나,
전송 순서를 제어하거나 흐름제어등을 수행하는곳


## choi님의 질문

VIP(Virtual IP)는 무엇인가?

```
virtual IP address: An IP address is assigned as a virtual IP address from the local subnet which is configured as a default gateway for all the local hosts.

로컬 서브넷이 사용하는 가상 IP.
내부의 모든 컴퓨터나 서버등은 위의 아이피를 통해 외부와 통신한다.

서비스의 VIP주소 한곳으로 외부의 요청을 받아들이고,
 로드밸런서를 통해 내부의 여러 목적지중 하나로 요청을 보내준다.

ip주소를 바꾸지 않고도 외부 요청을 처리하는 내부 서버를 변경할 수 있으므로 
로드밸런서와 함께 서비스의 가용성보장 및 부하 분산에 도움을 준다.
```

출처: https://www.geeksforgeeks.org/introduction-of-virtual-router-redundancy-protocol-vrrp-and-its-configuration/


## bee 님의 질문

L4 Load Balancing과 L7 Load Balancing의 차이

로드밸런싱은 데이터의 로드를 분배하는 역할을 한다

L4 와 L7은 각각 OSI7계층의 레벨을 의미


|구분|L4|L7|
|--|--|--|
|작동계층|전송계층|응용계층|
|부하분배 기준|ip주소/포트번호|HTTP헤더, 쿠키등 다양한 응용계층 정보|
|기능의 복잡도|단순함|복잡함 <br> 부하분배에 url패턴등을 이용하기도 함|

여러가지 래퍼런스에서 공통적으로 강조하는 부분은 L7 Load balancer는 굉장히 지능적이라는것.
데이터를 전송하는 곳을 application data를 해석하여 선택할 수 있음

https://medium.com/@crazy_nuclei/l4-vs-l7-load-balancers-64e47610e2ef


- 첨언

로드밸런서의 이름 그대로의 뜻과 역할 (부하 균등 분배기)
를 생각해보면 어쩌면 osi7계층의 맨밑에 있는 
네트워크 트래픽을 관리하는 스위치도 로드밸런서라고 할 수 있을것 같아 구글링 해보았는데

로드밸런서가 "분배기" 같은 느낌의 단어라 

찾아보니 실제로 교통관리나 전기 전력분야에서도 부하를 나누는 장치를
로드밸런서라고 표현하기도 하고 한다고합니다.

cs 깃허브에서 말한 로드밸런싱에대한 설명은 다음과 같습니다.

"클라이언트와 서버 사이에 두고, 부하가 일어나지 않도록 여러 서버에 분산시켜주는 방식"

하지만 응용계층에서 부하의 분배를 위해 사용하는 도구들을 뭉뜽그려서 
다 로드밸런서라고 부르는 경우가 잦다고 합니다.


## 클로이님의 질문

blocking과 synchronous는 동일한 것인가? 그렇지 않다면 그 이유에 대해서 설명하라.


동일하지 않음

이유: 둘 다 함수의 실행을 다루는 개념이지만 관심사가 다름

Blocking:

특정 함수의 실행이 다른 함수의 실행을 막는
 "제어권" 의 흐름에 초점을 맞추는 개념

Synchronous:

특정 함수들의 실행과 완료 구간, 
즉 "실행 타이밍"과 에 초점을 맞추는 개념

blocking 모델에서는 제어권을 가진 함수만이 실행될 수 있음
한 함수가 실행되고 다른것은 실행되지 않는다면, 현재 실행중인 함수가 제어권을 넘겨주지 않기때문

non-blocking 에서는 바로 넘겨줘서 그런일이 없음


synchronous 모델에서는 함수가 실행되고 끝나는 구간이 정해져있음
특정 함수가 실행될때, 다른 함수도 얼마든지 실행될 수 있음
그런데 같이 실행이 시작되어야함 

만약 같이 실행되지 않는다면 해당 구간에 실행된 함수들이 전부 끝나야 
다음 함수들이 실행될 수 있음


