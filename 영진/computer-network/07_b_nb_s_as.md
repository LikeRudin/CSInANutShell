# CONTENTS
```
    1. Blocking/Non-Blocking            
                        - Blocking
                        - Non-Blocking

    2. Synchronous/Asynchronous
                        - Synchronous
                        - Asynchronous

    3. Blocking/Non-Blocking IO
                        - Blocking IO
                            - Synchronous
                            - Asynchronous

                        - Non-Blocking IO
                            - Synchronous
                            - Asynchronous

```
---

# DETAILS

## 1. Blocking/Non-blocking

함수의 실행에서, `제어권`에 초점을 맞춘 개념.

다른작업의 실행이 현재 작업의 실행을 막는지/아닌지를 구분

- 사용예: 은행예금
    - 계좌금액의 변동을 저장하기전, 추가변동 불가


### Blocking

다른작업의 실행이 현재작업의 실행을 막음

- 작업중인 함수는 제어권을 가지고있음

- 특정함수가 제어권을 쥐고있으면 다른함수는 실행불가.

- 함수가 내부에서 다른함수를 호출시 제어권을 넘겨줌
    - 호출자는 실행을 멈추고, 피호출자는 실행됨

<br>

- 호출된 함수는 종료되고 제어권을 원래 함수에게 반환
    - 호출자가 다시 진행

<br>

```js
const outerFunction = () => {
    // 실행시 제어권이 outerFunction innerFunction으로 넘어간다.
    innerFunction(); 

    // innerFunction 종료되어 제어권이 outerFunction에 오기전에는 실행불가
    outerLogics(); 
}
const innerFunction = () => {
}
```

### non-Blocking

- 다른함수의 진행과 상관없이 진행

- 함수내부에서 다른함수를 호출해도 
바깥의 함수가 멈추지 않음



## 2. Synchronous/Asynchronous

작업의 시작과 끝나는 시각의 일치여부에 관심을 두는 개념

하나의 작업 사이클이 진행중일때, 다른 작업의 실행가능여부를 구분


### Synchronous

-  특정 시점에 동시에 시작한 작업들은 동시에 끝남 

- 구간 중간에 다른 작업이 실행될 수 없음

- 구간이 끝나야 다른 작업이 실행가능

### asynchronous 

- 작업의 시작점과 끝점과 상관없이 다른작업이 실행될 수있음

```js
// db와 통신하는 함수
const myAsynchronousFunction = () => {
    const user = sqlDataRepository.findOneById();
    return user //undefined가 반환됨
}
```

## 3.Blocking/Non-Blocking IO

https://developer.ibm.com/articles/l-async/
<img  src="
https://developer.ibm.com/developer/default/articles/l-async/images/figure1.gif"/>


### Blocking IO

- Synchronous
    ![](https://developer.ibm.com/developer/default/articles/l-async/images/figure2.gif)
    - 가장 흔하게 사용되는 모델

    - 사용자 application이 system call 호출시, application이 동작을 멈춤

    <br>

- Asynchronous
    ![](https://developer.ibm.com/developer/default/articles/l-async/images/figure4.gif)
    - blocking에 대한 사실을 공지하는 모델

    - I/O 처리가 즉시 완료되지 않을경우, 해당 사실을 사용자 application에 알림
        - 오류메시지 `EWOULDBLOCK` 사용

    - kernel 이 사용중임을 모든 application에 알릴 수 있는 장점이 있음

    <br>

### Non-Blocking IO

- Synchronous
    ![](https://developer.ibm.com/developer/default/articles/l-async/images/figure3.gif)
    - 잘 사용되지 않는 괴기한 모델

    - I/O 작업이 즉시처리되지 않는다면 오류메시지 `EWOULDBLOCK`를 전송
    
    - 사용자 application의 read요청과, kernel의 오류메시지 반복전송으로 시간을 포함한 컴퓨터자원을 낭비

    <br>

- Asynchronous
    ![](https://developer.ibm.com/developer/default/articles/l-async/images/figure5.gif)
    - 빠르고 사용자 경험이 좋은 모델
    - kernel은 요청한 I/O를 수행, 사용자 application은 자기 할일을 수행
    - kernel로부터 응답이 도착하면 해당 데이터가 필요한 작업을 수행


---

# QUESTIONS

자바스크립트는 비동기함수로부터 전달받은 데이터를 오류없이 사용하기위해 콜백, 프로미스, async/await 패턴등을 사용합니다.

이때, 운영체제에서 사용자 application이 kernel 에게 I/O 요청을 하는것을 자바스크립트가 비동기 함수로 외부 API에게 요청하는것과 같습니다.

### 1. 자바스크립트의 async /await 패턴, 콜백 패턴은 각각 어떤 IO 패턴과 유사한가요? 

### 2. Non-Blocking / Synchronous 패턴을 자바스크립트 비동기함수 처리를 이용하여 묘사해보세요

# ANSWERS

### 1.  자바스크립트의 async /await 패턴, 콜백 패턴은 각각 어떤 IO 패턴과 유사한가요?

async/await 패턴은 Blocking/Synchronous 패턴과 유사합니다

```js
const asyncAwait = async () => {
                // 외부 api 호출
    const data = await outerApiCall();
    useData(data)
}
```
위 코드에서 외부 api 호출 함수가 완료되기 전까지, `asyncAwait` 함수는 멈추게됩니다.
그리고 중간에 실행되는 함수도 없습니다.

이는 사용자 application이 제어권을 넘겨주고, kernel의 작업이 끝날때까지 사용자 application은 동작을 멈추는 Blocking/Synchronous 패턴과 유사합니다.

<br>

콜백 패턴은 Non-Blocking/Asynchronous 패턴과 비슷합니다

```js
const handleResponse = (response) =>{
    console.log(response.data);
}
callOuterApi(handleResponse);
otherFunction();
```

위 코드에서 callOuterApi 함수는 실행되면, 외부 api에게 응답을 요청합니다.

외부 api가 응답을 반환하지 않아도 otherFunction 을 실행하며 프로그램은 계속 진행됩니다.

그러다 와부 api의 응답값이 도착하면, 그것은 
사전에 할당해준 콜백이 처리하게됩니다.
외부 api실행이 함수의 실행을 멈추지도 않고,
응답처리가 동기적으로 오지도 않습니다.

물론 자바스크립트는 하나의 함수 콜 스택을 가지고 있기 때문에 완전한 non-blocking이라고 말할순 없습니다. 


### 2. Non-Blocking / Synchronous 패턴을 자바스크립트 비동기함수 처리를 이용하여 묘사해보세요


외부 api로 보낸 요청이 도착하기전에, 해당 요청을 저장할 변수를 사용하면 잘못된 데이터가 사용됩니다.

sql 내부에 쿼리를 넣었을때, 데이터가 없으면 null을, 있으면 데이터를 배열에 넣어 반환하는 함수가 있다고 합시다.

쿼리는 비동기 함수이므로, 쿼리가 완료전에 해당 변수를 사용하면 예측할 수 없는 값이 나올 수있습니다.
```js
const myAsynchronousFunction = () => {
    const users = sqlDataRepository.find();
    
    const customers = sqlDataRepository.find({where: {customer}});

    return users // 비동기 [] 반환
}
```

