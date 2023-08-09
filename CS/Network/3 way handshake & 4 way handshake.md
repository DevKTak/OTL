# 3 Way Handshake
> TCP 통신을 시작할 때 논리적으로 연결을 하기 위한 방식

<img width="585" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/d9a9f804-50af-4c81-971c-704ebb1458fd">

### 연결 과정
**[Step 1]**   
클라이언트는 서버에 연결을 요청하기 위해 **`SYN`** 을 전송합니다.

**[Step 2]**   
서버는 클라이언트로부터 **`SYN`** 을 받고 연결 요청을 수락한다는 의미로 **`SYN`**, **`ACK`** 를 전송합니다.

**[Step 3]**   
클라이언트는 서버로부터 받은 **SYN에 1을 더해 ACK에 저장**하고 **`ACK`** 를 서버로 전송합니다.    

> **요약: SYN -> SYN + ACK -> ACK**
 
---

# 4 Way Handshake
> TCP 통신을 종료할 때 논리적인 연결을 해제하기 위한 방식

## 4 Way Handshake 방식을 사용하는 이유
데이터 유실을 방지하기 위함입니다.

<img width="579" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/0c9fd09c-b73c-440f-ba02-f0243247f8c3">

### 해제 과정
**[STEP 1]**
클라이언트가 서버에 연결 해제 요청을 하기 위해 **`FIN`** 을 전송합니다.

**[STEP 2]**
서버는 연결 해제 요청을 승인한다는 의미로 클라이언트에 **`ACK`** 를 전송합니다. 요청이 바로 해제 되는 것이 아니고, CLOSE_WAIT 상태로 진입한 후 연결 해제 준비를 합니다.

**[STEP 3]**
서버는 연결 해제 준비가 완료되었다는 **`FIN`** 을 클라이언트로 전송합니다.

**[STEP 4]**
클라이언트는 **TIME_WAIT 시간만큼 대기 후 서버로 **`ACK`** 를 전송함으로써 마침내 연결이 해제됩니다.**
대개의 경우 2MSL(Maximum Segment Lifetime, 1분 ~ 4분)

> **요약: FIN -> ACK -> FIN -> ACK**


### 참고
> - [TCP, UDP 차이와 3-Way-Handshake, 4-Way-Handshake 이해하기](https://hpjang.tistory.com/4)   
> - [HTTP/3는 왜 UDP를 선택한 것일까?](https://evan-moon.github.io/2019/10/08/what-is-http3/?fbclid=IwAR1V1-yWjkzWEAqm_1OZfe_gtG05EuVo7WXXyVdEz_J0UHZBpGruU8PU0FY)
> - [TCP/UDP와 3-way-handshake/4-way-handshake](https://velog.io/@jsj3282/TCPUDP%EC%99%80-3-way-handshake4-way-handshake)
