# CONTENTS
```
    1. HTTP
        - 개요
        - 보안취약점
    2. HTTPS
        - TLS
        - CA
        - SSL/TLS handShake
```
---

## 1. HTTP 

`H`yper`t`ext `T`ransfer `P`rotocol

- 하이퍼텍스트 전송규약

- 클라이언트 - 서버 모델을 사용하는 요청-응답 프로토콜
    ![](https://media.geeksforgeeks.org/wp-content/uploads/20210905091508/ImageOfHTTPRequestResponse-660x374.png)
    - 요청-응답
        - 클라이언트가 HTTP request 전송
        ![](https://flylib.com/books/4/253/1/html/2/images/0131472275/graphics/17fig01.gif)

        - 서버가 HTTP response로 응답
        ![](https://media.geeksforgeeks.org/wp-content/uploads/20210905094321/StructureOfAHTTPResponse.png)

- TCP/IP 의 응용계층에서 작동

### 보안취약점

HTTP 자체는 개인 데이터 보안 기능이없다.
- 암호화 기능 X `평문`으로 전송

<br>

인터넷 서비스 제공자가 제어하는 라우터, 중개디바이스, 사용자 방문 웹사이트등은 전부 
이용자의 HTTP 요청 헤더, 발신내용 IP주소, 응답데이터등을 읽고 수정할 수있음

- 송/수신자 `위장`이나 메시지 `변조` 가능성 존재 


## 2. HTTPS


HTTP Secure (or HTTP + TLS)

```
발신 및 대상 IP 주소를 제외한 메시지와 HTTP 헤더를 암호화해서 전송하는 프로토콜
```

브라우저의 주소란 왼쪽에 자물쇠 아이콘이있다면 HTTPS를 이용중
- 눌러서 인증서 보기 가능

HTTPS를 이용중이지 않은 사이트 접속시
- 주의요함, 접근이 제한됨 페이지가 뜸
    - 그래도 접속하시겠습니까?


### TLS

HTTPS 에 사용되는 통신계층 보안 프로토콜


기능
- 프라이버시 확보

- 통신노드 ID 검증

- 메시지 무결성 점검
    - 분실 혹은 변조 예방


서비스 운영자 / 이용자가 아닌 신뢰할수있는 제 3자 인증기관 `CA`(Certificate Authorities) 활용

### CA의 역할

디지털 인증서를 서비스 운영자에게 발급


- 디지털인증서
    - 공개키가 진짜라는것을 확인하는 수단
    - 악의적인 3자가 자신의 공개키를 진짜인마냥 배포하여 서비스이용자들의 정보를 탈취하는것을 방지하기 위해 필요

    - 서버이름, 소유자ID, 공개키사본, CA 암호화 서명등을 담고있음
    - 서비스 운영자의 키 소유권을 증명함
    - 단일/복수 도메인이름에 발급가능


### 서비스 운영자가 HTTPS를 적용하는법

1. 공개키와 비밀키를 생성

2. 공개키를 `CA` 에게 전송, 인증서 발행신청 `CSR` (Certificate Signaling Request)

3. CA는 인증서 발행을 심사, 문제가없으면 인증서 생성

4. 생성한 인증서를 신청 서비스운영 조직에 발행

5. 발행받은 인증서를 이용 서버 등에 설치

### 이용:  SSL/TSL HandShake 과정



0. TCP 연결: 3-way Hand Shake 

1. Client Hello: <br>  클라이언트가 서버에 랜덤난수/ 가능한 암호화 방식 전송 

2. ServerHello:  <br> 서버가 클라이언트에게 랜덤난수/ 가능한 암호화 방식과 더불어 인증서 전송

3. 클라이언트의 브라우저는 서버로부터 받은 인증서를 검증
    - 브라우저는 서버의 신원을 확인하고, 해당 인증서가 신뢰할 수 있는 인증기관에 의해 발급된 것인지 확인

4. 그럼 위의 과정을통해 주고받은 난수를 조합하여 pre master secret 생성, 서비스 운영자의 `공개키`로 암호화 한후 서버에 전송

5. 서버는 pre master secret 를 복호화

5. 클라이언트의 브라우저와 서버는 각각 가지고있는 키를 통해 master secret key 생성

6. master secret 을 통해 session key(대칭키) 생성

7. 이후 session key를 통한 암호화를 거쳐 데이터를 송신, 복호화룰 거쳐 데이터를 수신

8. 통신종료시 session-key 파기


### TLS의 취약점

- CA의 훼손 가능성 존재
- 강제 위조인증서 발급 가능
    - 애플리케이션이 수락시 문제발생


## SSL/TSL HandShake 과정

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20201212102658/HTTP-Request-TCP-TLS.png)


![](https://www.exoprise.com/wp-content/uploads/ssl-setup-diagram.png)

---

# QUESTION


1. HTTPS를 사용하는 이유는 무엇이며 어떤 이점이 있나요?


2. 디지털 인증서의 역할은 무엇인가요?

---

# ANSWER

1. HTTPS를 사용해야 하는 이유와 이점에대해 설명해주세요


HTTP는 기본적으로 보안기능을 제공하지않으며, HTTPS를 사용시 다음과 같은 이점을 얻을수있습니다.

| 이점 | 설명|
|--|--|
|기밀성|데이터를 암호화하여 중간에 누출되는 것을 방지|
|무결성| 데이터가 변조되지 않았음을 보장|
|신원 인증|서버의 신원을 확인하여 중간자 공격을 방지|
|사생활 보호|사용자의 개인 정보를 보호|
|검색 엔진 노출|검색 엔진에서 HTTPS 사이트를 우선적으로 표시|


2. 디지털 인증서의 역할은 무엇인가요?

공개키 암호화 방식을 사용할때, 송신자는 공개키가 정말로 자기가 데이터를 전송하려하는 서비스가 공개한 키인지 확인할 필요가있습니다.

악의적인 제 3자가 자신의 공개키를 해당 서비스의 키처럼 배포하고, 그것을 데이터 송신자가 사용시 개인데이터가 유출되기 때문입니다.

디지털인증서는 공개키가 정말로 서비스 운영자의 공개키인지 신원확인을 하는 역할을 해줍니다.