# CONTENTS

```
    1. TCP/IP 계층구조
        - 개요 및 특징

        - A.응용계층
        - B.전송계층
        - C.네트워크계층
        - D.데이터링크계층

    2. 주소
        - 인터넷 주소
            - 계층별 주소구분
            - 인터넷 주소 형태
            - 사설 네트워크 주소
            - 서브네팅

        - 데이터 송수신 규칙
            - 캡슐화

    3. IP
        - 개요
        - IP헤더 구조
        - 단편화
        - 라우팅
        - IPV4 vs IPV6

    4. UDP
        - 특징
        - 데이터그램 구조

    5. TCP
        - 특징
        - 데이터그램 구조
        - 데이터 전송절차
            - 연결설정과정
                - 3-way handshake
                - 4-way handshake
```
---

# DETAILS

## 1. TCP/IP 계층구조

![](https://1.bp.blogspot.com/-ZUB_-spqu3c/YD5aQJXDqlI/AAAAAAAACMU/tfbvsSJVaYw51k4xwNTLt4ZGj947Kfy1ACLcBGAsYHQ/s617/tcp.PNG)

`T`ransmission `C`ontrol `P`rotocol/`I`nternet `P`rotocol 


- 네트워크 아키텍쳐의 일종으로, 데이터 통신에 필요한 프로토콜의 집합을 계층화/ 컴포넌트화 하여 분리한 구조

    - 프로토콜: 주소나 데이터형식, 통신절차등 통신에 필요한 규칙

    - 네트워크 아키텍쳐: 프로토콜의 집합    
    <br>

### TCP/IP 모델의 특징

- 프로토콜의 변경 및 확장이 용이
- 호스트간의 통신은 동일 계층간 통신으로 이루어짐


![](https://microchip.wdfiles.com/local--files/tcpip:tcp-ip-five-layer-model/tcpip_5_layer_overview.png)


- 현재 실무에서 주로 사용되는 계층구조
    - 처음부터 실용성을 목적으로 제작
    - 스마트폰 / PC 전부 TCP/IP 모델을 사용
    - OSI 7계층은 거의 사용하지 않음




### A. 응용계층(Application)

- 애플리케이션에서 다룰 데이터 형식 및 절차를 결정
    - 비트에 저장된 0과 1을 넘어서 문자나 이미지데이터를 인식 할 수있게 함

<br>

- 사용자 및 응용 프로그램에 직접 연관, 네트워크 접근 수단을 제공 


대표적 프로토콜
|이름|기능 및 특징|
|--|--|
|HTTP(S):<br> Hypertext transfer protocol (Secure)|웹 브라우저와 서버간 통신을 관리|
|NTP: <br> Network Time Protocol|하나의 기준 시에 각 컴퓨터의 시각을 동기화|
|FTP(S):<br> File Transfer Protocol(Secure)|컴퓨터들간 파일교환|
|TELNET:<br> teletype network|사용자가 원거리 컴퓨터를 조작할 수 있도록 함|
|DHCP:<br> Dynamic Host Configuration Protocol|통신준비: 컴퓨터에게 IP주소를 분배|
|PING: <br> Packet Internet Groper|상대간 연결 가능여부 확인/ 점수가 높을수록 안좋다|

### B. 전송계층(Transport)

전송계층은 컴퓨터가 수신한 데이터를 여러 애플리케이션의 목적에 맞게 배분하는 역할


- 한 PC로 들어오는 데이터도 도착 애플리케이션은 다름
  - 온라인 게임서버에서 pc로 전송한 데이터가 웹 브라우저로 가면 곤란


PORT주소를 관리
- 컴퓨터 내에서 애플리케이션을 식별하는 주소
- node.js로 로컬 서버 실행할때 언급하는 PORT와 동일개념




대표적 프로토콜
|이름|기능 및 특징|
|--|--|
|TCP: <br> Transmission Control Protocol|물리적 회로를 구성하여 문자단위로 데이터전송|
|UDP: <br> User Datagram Protocol|회로 없이 데이터그램단위로 데이터전송|


-  데이터 명세 교환 및 패킷 누락문제를 해결, 호스트간 데이터 전송 제공
-  데이터의 분할 및 조립


### C. 네트워크계층(Network) === 인터넷계층(Internet) 

인터넷환경은 다양한 네트워크의 조합

![](https://ars.els-cdn.com/content/image/3-s2.0-B9780123744944000153-f15-06-9780123744944.jpg)

네트워크계층은 네트워크간 데이터 전송에 필요한 논리적 기능을 담당
- 네트워크를 구별하는 논리주소 지정, 네트워크간 라우팅
- 네트워크 연결 유지/설정/해제

대표적 프로토콜

|이름|기능 및 특징|
|--|--|
|IP: <br> Internet Protocol|데이터 패킷의 헤더에 들어있는 IP주소를 통해 패킷을 송신자로부터 수신자에게 전달|
|ICMP: <br> Internet Control Message Protocol|IP 데이터그램을 감싸서 통신정보 전달 및 네트워크 문제 대응|
|ARP: <br> Address Resolution Protocol|IP주소를통해 하드웨어 주소 탐색|

- End to End 통신: `원격 네트워크간` 송신지와 목적지 간 데이터 전송
    - End to End통신의 신뢰성보장: 데이터가 오류없이 전송됨을 보장

- IP는 통신에 직접이용, ICMP, ARP등은 IP를 보조

### D. 링크계층(Network Interface or Network Access)

같은 네트워크 내부에서도 데이터 전송 규약이 필요

![](https://3.bp.blogspot.com/_Ede6sUzdNz0/TOVYK-yio5I/AAAAAAAAABI/91Uhmt1cEe0/s640/Simple+Hub-and-spoke.png)

링크계층이 한 네트워크 내부에서의 데이터 전송 및 통신에 필요한 기능을 담당
- 비트전송
- 비트를 가공하여 프레임을 전송
- 노드를 구별하는 물리주소 지정
- 네트워크 구성

대표적 구성요소

|이름|기능 및 특징|
|--|--|
|EtherNet|데이터 패킷과 네트워크 구성을위한 프로토콜 CSMA/CD 사용|
|Fiber|데이터 전송 물리 케이블  |
|Switch|여러 기기를 연결하여 네트워크를 구성|
|Router|여러개의 switch를 연결하여 더 큰 네트워크를 구성|




## 2. 주소


### 인터넷 주소


- 계층별 주소 구분

    |이름|계층|기능|
    |--|--|--|
    |물리주소|링크<br>(Network Interface) |하나의 네트워크 내에서 호스트를 식별하는 물리적 하드웨어 주소(네트워크 인터페이스 주소)|
    |인터넷주소|네트워크<br>(Network)|패킷이 네트워크를 통해 전달될 때, 네트워크간 호스트를 식별하는 논리주소|
    |포트주소|전송<br>(Transport)|프로세스를 식별, 응용 프로그램이 특정 네트워크 서비스에 접근하기 위해 사용|

- 인터넷 주소 형태

    - 표기법
        - 도트형십진표기

          ![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/IP_addressing_1.jpg)
          - IPv4에 이용
          - 8비트씩 잘라 십진수로 변환
          - `.`으로 단위를 구분
          - 8비트로 나타낼 수있는 수는 0부터 255까지
          - 256이상의 수치가 포함된것은 IP주소가아님
          
          <br>

        - 십육진수 표기

            ![](https://media.geeksforgeeks.org/wp-content/uploads/20230620171208/1212.jpg)

            - IPv6에 이용

    <br>
    
    - 목적지 개수에 따른 주소
        - 목적지 개수에따라 주소가 다름, 교과서들에선 주로 유니캐스트만 다룸

        - 멀티캐스트: 특정 그룹에 포함되는 호스트에 완전히 똑같은 데이터를 전송하는것(단톡방)
            - `from 224.0.0.0 to 239.255.255.255`
        
        <br>

        - 브로드캐스트: 같은 네트워크상의 모든호스트에 완전히 똑같은 데이터를 전송하는것: (애플리케이션공지)
            - `255.255.255.255`
            - 호스트부분을 전부 1로 채움

        <br>

        - 유니캐스트: 단 한곳으로 데이터를 전송하는것
            - 네트워크를 식별/ 내부호스트 식별부분으로 나뉨
        
        
        <br>


    - 클래스화된 인터넷주소
    
        - 각 주소를 범위별로 클래스화 한 주소
        - 네트워크 식별자와 호스트 식별자의 경계를 표현하는 방법, 약속
    ![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/IP_addressing_3.jpg)
        
        |클래스|시작비트 =클래스 결정비트|from|to|
        |--|--|--|--|
        |A|0|0-0-0-0|127-255-255-255|
        |B|10|128-0-0-0|191-255-255-255|
        |C|110|192-0-0-0|223-255-255-255|
        |D|1110|224-0-0-0|239-255-255-255|
        |E|1111|240-0-0-0|247-255-255-255|
        
        - 최초 4비트로 어떤 클래스에 속하는지 알 수있음

        - 4개의 바이트를 각각 옥텟이라 부름

      
    



### 사설 네트워크 주소

전체 인터넷이아닌 특정 기관 내 네트워크에서만 사용하기위한 내트워크 주소

- 외부에서는 유효하지 않음
- 다른네트워크와 주소가 겹처도 문제없음


### 서브네팅

하나의 주소에 포함된 네트워크/호스트 주소를 구분하기위해서 내부 비트 위치를 이용해 주소를 분할하는것

서브넷(subnet) 본래 하나인 네트워크를 작은 단위로 분할한것

- 서브넷 마스크: IP주소에서 어디까지가 네트워크고 어디서부터 호스트인지 나타내는 비트
    - 네트워크와 호스트부 구분
    - `1`은 네트워크 `0`은 호스트부
    - IP주소와 똑같은 갯수의 비트
    

![](https://www.freecodecamp.org/news/content/images/2021/04/network-and-host-bits-2.png)

### 데이터 송수신 과정

통신주체인 애플리케이션간 데이터를 주고받게 하려면, 복수의 프로토콜 조합이 필요

각 프로토콜에는 기능을 실현하기위한 제어정보가 필요
- 제어정보는 헤더에 포함된다.


캡슐화: 헤더를 추가하는것


<div style="display: flex; flex-direction: row;">
  <div style="flex: 1; margin-right: 20px;">
    <table>
      <thead>
        <tr>
          <th>이름</th>
          <th>설명</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>PDU: <br> Protocol Data Unit</td>
          <td>상위계층에서 내려온 데이터</td>
        </tr>
        <tr>
          <td>PCI: <br> Protocol Control Unit (header)</td>
          <td>데이터에 붙여줄 제어정보가 포함된 헤더</td>
        </tr>
        <tr>
          <td>SDU: <br> Service Data Unit</td>
          <td>PCI + PDU</td>
        </tr>
      </tbody>
    </table>
  </div>
  <div style="flex: 1;">
    <img src="https://static.packt-cdn.com/products/9781789340501/graphics/assets/4413e8e2-d953-4431-af99-1f96ce07e8b4.png" />
  </div>
</div>





### 웹브라우저 데이터 전송과정



### 1. 캡슐화 encapsulation


데이터 송신측에서 헤더를 덧붙이며 데이터를 하위계층으로 이동시킨다.

- ![](https://www.researchgate.net/profile/Isara-Anantavrasilp/publication/49288737/figure/fig4/AS:669528941924353@1536639547060/Packet-encapsulation-TCP-IP-architecture-encapsulates-the-data-from-the-upper-layer-by.png)


```
0. 데이터 선택
1. 응용계층: HTTP 헤더 추가
2. 전송계층: TCP 헤더추가
3. 네트워크계층: IP 헤더추가
4. 링크계층: Ethernet 헤더추가/ FCS 추가
```

- FCS(Frame Check Sequence) 오류제어를 위한 정보

### 2. 데이터 전송
- 네트워크 기기를 거쳐 목적지 웹서버로 전송

### 3. 역캡슐화 decapsulation

수신자는 도착한 데이터에서 각 계층별로 헤더를 떼어내며 데이터를 상위계층으로 이동시킨다.

### 참고 그림
<div style="display: flex; flex-direction: row;">
  <div style="flex: 1; margin-right: 20px;">
    <img src="https://docs.oracle.com/cd/E19683-01/806-4075/images/ipov.fig88.epsi.gif" />
  </div>
  <div style="flex: 1;">
    <table>
      <thead>
        <tr>
          <th>계층</th>
          <th>데이터단위 이름</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>응용계층</td>
          <td>메시지</td>
        </tr>
        <tr>
          <td>전송계층</td>
          <td>세그먼트</td>
        </tr>
        <tr>
          <td>인터넷계층</td>
          <td>패킷</td>
        </tr>
        <tr>
          <td>링크계층</td>
          <td>프레임</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<br>

![](https://media.geeksforgeeks.org/wp-content/uploads/20210905102728/Encap.png)


## 3. IP

### 역할


네트워크상의 기기에 `주소를 할당`하거나, 해당 주소로 패킷을 전송할 수 있게 함

- End-To-End 통신: 네트워크상의 어떤 PC에서 다른 PC로의 데이터전송
  
상위계층으로부터 전달받은 `데이터`에 IP 헤더를 추가해 IP 패킷을 만듦

- `데이터` = 응용계층 데이터 + 응용계층 헤더 + 전송계층 헤더 
- = TCP나 UDP에서 보낸 데이터

### 특징

중복전송, 패킷손실등 오류에 신경쓰지 않음 
- 비신뢰성(unreliable)

데이터를 보내기위해 회로를 구성하지 않음
- 비 연결성(connectionless)






### IP 헤더
![](https://media.geeksforgeeks.org/wp-content/uploads/20220222105138/geekforgeeksIPv4header.png)

|파트이름|비트 수|설명|
|--|--|--|
|Version(버전)|4|IP프로토콜 버전|
|HeaderLength(헤더길이)|4|데이터그램 헤더의 길이|
|Type of Service(서비스유형)|8|우선순위/서비스 품질조건: 비용,신뢰성, 처리량, 지연등|
|Total Length(전체길이)|8|데이터그램의 전체길이|
|Identification(식별)|16|단편화시 각 데이터그램을 위한 고유 식별값|
|Flags(플래그)|3|여유비트/단편화허용여부/마지막단편여부|
|Fragment Offset(단편오프셋)|13|단편화전 데이터그램에서의 위치|
|Time To Live(수명)|8|데이터그램 활동시간|
|Protocol(프로토콜)|8|상위계층 프로토콜|
|Header Checksum(헤더검사합)|16|전송오류 제어를 위한 검사합|
|Source IP address(발신지 IP주소)|32|발신지 주소|
|Destination IP address|(목적지 IP 주소)|32|목적지 주소|
|Option(옵션)|[0, 40]|라우팅, 보안등의 제어를 위한 옵션|


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F954Xv%2FbtqZJ5uQr2x%2FjjNQGJKsWgaJuKIgY2Gb80%2Fimg.png)

- 중간에 있는 라우터가 IP 헤더를 참조하여 IP 패킷을 전송

### IPv6 vs IPv4

### IPv6

 패킷처리 오버헤드를 줄이기위해 새로운 헤더를 도입하고, 주소에 사용되는 비트를 4배 늘린 128비트 주소체계 


### 도입이유

- 클래스 형식으로 부여하는 IPv4주소가 전 지구에서 사용하기에 충분치 않음

- IPv4의 한계를 보완할 새로운 주소형식의 필요

- 그동안은 여러도구로 IPv4의 취약점을 보완하며 버텨옴
    - CIDR: 클래스표기 방식을 대체
    - DHCP: IP주소를보관, 필요한 클라이언트에 대여
        - 고정IP가 아닌 유동IP사용
    - NAT:  사설IP주소를 공인IP로 변환하는 기술
        - 소규모 네트워크 내부에선 사설 IP주소를 사용하여 공인IP주소 고갈문제에 대처, 필요할떄만 공인IP주소를 사용 




### IPv4와의 비교

|요소|IPv6|IPv4|
|--|--|--|
|속도|더 빠름 |느림|
|주소공간비트|128|32|
|보안기능|내장|IPSec 미설치시 없음|
|표기법|십육진수|십진도트|

### 헤더구조
![](https://www.networkacademy.io/sites/default/files/inline-images/comparing-ipv4-and-ipv6-headers.png)



## 4. UDP

`U`ser `D`atagram `P`rotocol

### 역할

애플리케이션에 데이터를 배분하기 위해 이용하는 프로토콜

### 특징

통신회로 설립없이 데이터를 전송

신뢰성 보장 X

소규모의 데이터 전송시 TCP보다 속도가 훨씬 빠름
  - 영상 스트리밍이나 게임에 많이 이용

### UDP 헤더

![](https://media.geeksforgeeks.org/wp-content/uploads/UDP-header.png)

|이름|비트 수|설명|
|--|--|--|
|Source Port(발신지 포트)|16|발신포트 식별자|
|Destination Port(목적지 포트)|16|목적포트 식별자|
|Length(데이터그램 길이)|16|헤더와 데이터 전체의 길이|
|Checksum(검사합)|16|옵션필드: 헤더 및 데이터 필드 오류제어에 이용|

## 5. TCP


`T`ransmission `C`ontrol `P`rotocol


### 역할

응용계층의 통신데이터를 수신, 패킷으로 분할후 네트워크계층으로 전달

애플리케이션간의 신뢰성있는 데이터전송을 보장

- TCP를 이용시, 애플리케이션에 따로 신뢰성을 위한 구조를 넣을 필요가 없음

### 특징

수신확인 / 패킷 재송신 절차가 있으므로 UDP에 비해 느림

데이터 전송을 위한 회로구성 필요

### TCP 헤더


![](https://media.geeksforgeeks.org/wp-content/uploads/TCPSegmentHeader-1.png)

|이름|비트 수|설명|
|--|--|--|
|Source Port(발신지 포트)|16|발신포트 식별자|
|Destination Port(목적지 포트)|16|목적포트 식별자|
|Sequence Number(순서 번호)|32|분할된 패킷의 순서를 나타내는 비트, 신뢰성 및 흐름제어 기능에 이용|
|Acknowledgement Number(확인응답/승인 번호)|32| 다음에 수신될 바이트 번호 <br> === <br> 마지막에 수신된 순서번호 +1 |
|Header Length(헤더길이)|4|TCP 헤더 길이|
|Reserved(예약)|6|추후 사용될 서비스를 위한 여유필드|
|Control Flags(제어플래그)|6|`URG`: Urgent Pointer 필드유효여부  <br>  `ACK`: Acknowledgement number 필드유효여부 <br> `PSH`:Request for push-수신자는 세그먼트 데이터를 빠르게 상위계층에 전달해야함 (ASAP) <br> `RST`: Reset the connection 연결을 재설정함 <br> `SYN`: Synchronize sequence numbers Sequence Number필드 동기화 - 연결설정 요청 or 연결 초기화를 위함 <br> `FIN`: 데이터 전송 종료여부 (마지막 데이터 여부)  |
|Window Size(윈도우 사이즈)|16| 수신자 흐름제어 윈도우 사이즈(바이트크기)|
|Checksum(검사합)|16|전체 세그먼트 오류제어용|
|Urgent Pointer(긴급포인터)|16|세그먼트에 포함된 긴급 데이터의 마지막 바이트에 대한 일련번호|
|Option(옵션)|[0,40]|발신자가 수신을 원하는 세그먼트의 최대 크기 `MSS`, 윈도우 증가크기값 등 기타 옵션을 나타냄|

### TCP에 의한 데이터 전송절차

1. TCP 연결설정
    - 3-way hand shake
2. 데이터 송수신
3. TCP 연결해제
    - 4-way hand shake

### 연결구성 / 해제 절차

- TCP는 UDP와 다르게 송신지로부터 수신지까지의 물리적 회로를 통해 데이터를 전송
  
    - 연결 설정 및 해제 과정이 필요

### 3-way hand shake

데이터 전송을위한 연결 (= 동기화 = 회로구성)을 하는 과정


![](https://qph.cf2.quoracdn.net/main-qimg-fa6ea10bec3d6f20a2d40f52e9615dfb-lq)



1. 클라이언트:  `SYN` 동기화 요청 패킷 
전송

2. 서버:  `SYN-ACK` 동기화 확인 패킷 전송

3. 클라이언트: `ACK` 확인 패킷 전송

- 이후 데이터 전송

### 4-way hand shake

모든 데이터 전송이 성공적으로 완료된후 회로에 사용된 자원들의 연결을 해제하는 과정

![](https://media.geeksforgeeks.org/wp-content/uploads/CN.png)

1. 클라이언트: `FIN` 패킷 전송후 `FIN_WAIT_1` 상태로 진입

<br>

2. 서버: FIN 수신 즉시  `ACK` 확인 패킷을 전송후 `CLOSE_WAIT` 상태 진입
    - `ACK` 패킷을 수신한 클라이언트는 FIN_WAIT_2 상태로 진입

<br>

3. 서버: 작업완료후 `FIN` 패킷을 전송

<br>

4. 클라이언트: `ACK` 전송후 `TIME_WAIT` 상태에 진입
    - `TIME_WAIT` 시간만큼 대기후 연결 종료

### 참고그림:  4-way hand shake 순서전이도

<div style="text-align:center;">
  <div style="flex: 1; margin-right: 10px;">
    <h3>클라이언트 관점</h3>
    <img src="https://media.geeksforgeeks.org/wp-content/uploads/CN-1.png" alt="Image 1" width="100%" />
  </div>
  
  <br>

  <div style="flex: 1; margin-left: 10px;">
    <h3>서버관점</h3>
    <img src="https://media.geeksforgeeks.org/wp-content/uploads/CN-2.png" alt="Image 2" width="100%" />
  </div>
</div>

---

# QUESTION

-  서브네팅에 대해 설명해주세요. 
    - 설명에는 서브넷 및 서브넷 마스크의 정의와 기술의 전체적인 목적및 사용법이 포함되어있어야 합니다.
---

# ANSWER


### 서브넷 마스크

서브넷 마스크는 IP주소의 `네트워크부분`과 `호스트부분`을 구분지어 표현하는 비트의 나열입니다.

네트워크부분을 연속된 1로 표현하고, 호스트부분을 0으로 나타냅니다.

![](https://miro.medium.com/v2/resize:fit:1400/1*xI4kfW-YYXCkWbZEi_LFzQ.png)

위의 사진하단의 표의 첫번째 행에 위치한 A의 서브넷 마스크를 보세요.

앞의 옥텟 3개`11111111 | 11111111 | 11111111`를 통해 A의 IP주소중 해당부분은 네트워크 부분, 뒤의 옥텟 `00000000` 은 호스트부분임을 알 수있습니다.

이런 `서브넷 마스크`를 이용해서 네트워크하나를 내부에서 자체적을 작은 여러개의 네트워크로 분할하는것을 `서브네팅`, 분할된 네트워크를 `서브넷` 이라고합니다.

### 예시: 클래스 B IP 주소 서브네팅

클래스B는 앞의 16비트를 네트워크부분으로 사용합니다.

- 클래스 B IP 주소의 범위
  |10|128-0-0-0|191-255-255-255|
  |--|--|--|


어떤 기관이 다음과 같은 주소의 네트워크를 할당 받았다고 합시다.

 `175.200.0.0` ~`175.200.255.255`

이 기관의 네트워크부분 주소는 `175.200` 입니다.
뒤의 `0.0`부터 `255.255` 까지의 주소를 각 호스트에게 할당 할 수 있습니다. 

그런데 해당 기관은 여러개의 시설로 나누어져있었기에
관리자가  각 시설을 하나의 작은 네트워크로 만들어주고 싶었습니다.

그래서 서브넷 마스크를 `11111111 | 11111111 | 11111111| 00000000` 로 정의했습니다.

그러면 이 기관 내부에서는 
`175.200.0` 부터 `175.200.255` 까지 `256`개의 하위 네트워크(서브넷)주소가 생성되고 각 네트워크는 `0` 부터 `255` 까지 256개의 호스트주소를 갖게됩니다.

이렇게 서브네팅을 해준다면, 네트워크안에 모든 메시지를 보내는 `브로드캐스트` 같은 작업을할때, 필요없는 호스트에게 메시지를 전달하지 않게 정밀한 선택을 할 수 있습니다.


### 더 알아보기
 
클래스기반 IP주소체계에서는 서브넷 마스크가 별로 중요하지않았습니다.

![](https://i1.faceprep.in/feed/images/intro-and-classful-addressing-image1.webp)

위의 사진처럼 클래스기반 `classful IP`주소는 시작부분의 4비트로 IP주소가 속하는 클래스를 알 수 있고, 그를 통해 약속된 네트워크부분과 호스트부분을 명확히 파악할 수 있기 때문입니다.


하지만 각 기관별로 호스트 규모가 천차만별인것에 비해
`classful IP`는 호스트의 갯수를 고정하는 한계를 가지고있습니다
- 호스트 주소를 나타내는 개수는 클래스별로 8 ,16, 32비트로 고정

`175.200`으로 클래스 B 네트워크 주소를 할당받았으면
뒤의 16비트로 호스트 주소를 나타냅니다. 

해당 네트워크는 6만개이상의 호스트 주소를 할당받은것입니다.

하지만 실제로 호스트가 2000개밖에없으면 그만큼의 IP주소를 낭비하게됩니다.

그래서 좀 더 다양한 경우를 통해 네트워크와 호스트부분을 구성하게됨에따라  `서브넷 마스크` 도 덩달아 중요해졌습니다.

