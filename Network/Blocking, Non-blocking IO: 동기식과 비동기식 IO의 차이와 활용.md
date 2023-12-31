# Blocking, Non-blocking I/O: 동기식과 비동기식 I/O의 차이와 활용

## I/O란?

> 데이터의 입력(Input)과 출력(Output)을 함께 일컫는 말
> 
- 네트워크를 통해 다른 서버와 데이터를 주고받는 경우, 디바이스, 콘솔 I/O 등
- CPU와 I/O 장치 간의 성능 차이는 종종 병목 현상을 일으킨다. CPU는 매우 빠르게 작동하지만, I/O 장치는 상대적으로 느리기 때문에  I/O 작업을 기다리는 동안 프로그램이 CPU를 사용하지 못한다.
- 따라서 I/O가 많아지면 애플리케이션이 연산을 할 때 대기시간이 많아지고 애플리케이션 처리 속도도 느려진다.

공통적으로 I/O는 user-space aplication 에서 직접 수행될 수 없다. I/O를 수행하기 위해서는 **커널에 한번 이상 시스템 콜을 보내야 한다.**

시스템 콜을 보내게되면 그 순간에 커널로 제어권이 넘어가고(context-switch), 유저 프로세스(or 스레드)는 제어권이 다시 돌아오기 전까지 block이 된다. 쉽게 말하자면 block 이 되어 있는 동안 유저프로세스는 다른 작업을 하지 못하게 된다.

![image](https://github.com/05AM/CS-Chanmi/assets/83827023/6eb9721f-a954-424b-9575-26462847378c)

<br>

## ☀ Blocking과 Non-Blocking

**호출되는 함수가 제어권을 바로 return하는가**

- Blocking과  Non-Blocking은 함수 호출에서 호출자의 태도
- 호출된 함수가 호출한 함수에게 제어권을 바로 주는지, 주지 않는지의 차이이다.

### **Blocking** I/O

> I/O작업이 진행되는 동안 프로세스가 작업 완료를 기다리며 다른 작업을 수행하지 못하고 대기하는 동기적 데이터 처리 방식
> 
- A 함수가 B 함수를 호출할 때, B 함수가 자신의 작업이 종료되기 전까지 A 함수에게 제어권을 돌려주지 않는 것
- 작업을 요청하면 일단 요청한 쪽은 일단 block이 되고, 작업이 완료가 된후에 응답을 받을수 있음. 그렇기에 완료가 되기 전에는 요청한 쪽은 block이 되어 다른 작업을 수행하지 못함.

### **Non-blocking I/O**

> 프로세스가 I/O 작업을 요청한 후 즉시 제어를 되찾아 다른 작업을 계속 수행할 수 있는 비동기적 데이터 처리 방식
> 
- A 함수가 B 함수를 호출할 때, B 함수가 제어권을 바로 A 함수에게 넘겨주면서 A 함수가 다른 일을 할 수 있도록 하는 것
- 작업을 요청하면, 즉시 응답이 돌아옴

## ☀ **동기와 비동기**

**호출되는 함수의 작업 완료 여부를 누가 신경쓰는가**

- 호출된 함수의 종료를 호출한 함수가 처리하느냐, 호출된 함수가 처리하느냐의 차이이다.

### **동기 (Synchronous)**

> A 함수가 B 함수를 호출 할 때, B 함수의 결과를 A 함수가 처리하는 것
> 
- **동기**란 요청과 그 결과가 동시에 일어난다는 것으로, 바로 요청을 하면 결과가 나올때까지 기다렸다가 해당 요청을 먼저 처리하고 다음 요청으로 넘어간다.
- 작업을 요청한 측에서 작업의 완료 여부를 체크한다**.**
- 설계가 간단하고 직관적이지만, 결과가 주어질 때까지 다른 요청은 수행하지 못하고 대기해야 한다는 단점이 있다.

### **비동기 (Asynchronous)**

> A 함수가 B 함수를 호출 할 때, B 함수의 결과를 B 함수가 처리하는 것 (callback)
> 
- **비동기**란 요청과 결과가 동시에 일어나지 않는 것으로, 요청이 들어오고 시간이 걸리면 다음 요청부터 처리하고 이전 요청이 끝나면 그 결과를 전달한다.
- 작업을 요청 받은 측에서 작업의 완료 여부를 알려준다.
- 동기보다 복잡하지만, 요청에 대한 결과 처리가 오래 걸리면 다른 작업을 할 수 있으므로 자원을 효율적으로 사용할 수 있다.

<br>

## ☀ Sync blocking I/O

- 시스템 콜이 들어오면, 커널은 IO 작업이 **완료되기 전에는 응답을 하지않는다.**
- 즉 IO 작업이 완료되기전에는 제어권을 커널이 갖고있는다
- 그렇기에 시스템 콜을 보낸후에, 유저 프로세스는 응답을 받기 전에는 block이 되어 다른 작업을 하지못한다. 즉 **IO 작업이 완료되기 전에는 다른 작업을 수행하지 못한다.**

<br>

## ☀ Async non-blocking I/O

- 시스템 콜이 들어오면, 커널은 IO 작업의 완료 여부와는 무관하게 **즉시 응답을 해준다.**
- 유저 프로세스는 I/O 가 완료 되기 전에도 다른 작업을 할 수 있다.
- I/O 처리는 백그라운드에서 실행되다가, **완료 시 커널이 유저 프로세스에게 알려준다.**
- `sync nonblocking I/O`는 유저 프로세스가 I/O 완료 여부를 커널에게 계속 물어봐야 했지만, `async non-blocking I/O`는 완료가 되면 그때 커널이 유저프로세스에게 알려주는 방식이다.

<br>

![image](https://github.com/05AM/CS-Chanmi/assets/83827023/20e7c764-f482-4e91-8a40-19fc63cb99cf)

<br>

## ☀ Sync non-blocking I/O

![image](https://github.com/05AM/CS-Chanmi/assets/83827023/da447f11-bcbe-4300-abf7-d3c6a27ffa22)

- 시스템 콜이 들어오면, 커널은 IO 작업의 완료 여부와는 무관하게 **즉시 응답을 해준다.** (완료되지 않았다면 에러 코드로 응답)
- 커널이 시스템 콜을 받자마자 제어권을 다시 유저 프로세스에게 넘겨주므로, **유저 프로세스는 I/O 가 완료되기 전에 다른 작업을 할 수 있다.**
- 유저 프로세스는 다른 작업들을 수행하다가 중간 중간에 시스템 콜을 보내서 IO가 완료되었는지 커널에게 물어 본다.

<br>

## ☀ Async blocking I/O

![image](https://github.com/05AM/CS-Chanmi/assets/83827023/ca8a1657-7764-4ed8-8329-d56165894a4e)

- I/O는 non-blocking이지만, 통지(notification)는 blocking 방식으로 하도록 되어있다.
- select() 는 유저 프로세스를 block 한다.
- select() 는 데이터가 사용이 가능해면 통지를 받게된다. 통지를 받으면 block를 푼다.

![image](https://github.com/05AM/CS-Chanmi/assets/83827023/a5a70158-c381-4639-8b7a-0e72bd3cf5f7)

![image](https://github.com/05AM/CS-Chanmi/assets/83827023/f980f971-2dad-477e-91ae-4b3069c1a9ad)

<br>

## ☀ 참고 자료

[동기와 비동기, 그리고 블럭과 넌블럭](https://musma.github.io/2019/04/17/blocking-and-synchronous.html)

[3주차 개념 스터디 - Javascript의 비동기 기술](https://yummy0102.tistory.com/116)

[Blocking, Non-blocking, Sync, Async 의 차이](https://jh-7.tistory.com/25)

[blocking, non-blocking IO, 동기, 비동기 개념 정리](https://limdongjin.github.io/concepts/blocking-non-blocking-io.html#특정-블로그-자료에-따른-분류)


