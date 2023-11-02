
<img width="807" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/5730166e-d6ca-4c98-a8bb-765d9bbe8c72">

동기: 작업의 대한 순서가 보장되는 것

**`블로킹/논블로킹`이** **제어권 반환** 에 중점을 두었다면,
**`동기/비동기`는** **결과를 바로 주는 지** 에 초점을 맞추면 이해할 수 있습니다.

**`동기`는 작업 결과값을 직접 받아 처리하는 반면**, **`비동기`는 결과값을 받으면 어떻게 할지의** **`콜백함수`** 를 미리 정의해둡니다.

### Synchronous && Blocking
- 동기 방식의 블로킹은 수행한 대로 순서에 맞게 수행됩니다.
- ex) `read()`, `write()`

### Synchronous && Non-Blocking
- 동기 방식의 논블로킹은 작업을 시작하고 제어권을 다시 돌려주기 때문에 다른 작업을 수행할 수 있습니다.
- 종료 시점은 Application 단의 Process 혹은 Thread가 Polling(지속적인 완료 확인)을 합니다.

### Asynchronous && Blocking
- 비효율적인 방식

### Asynchronous && Non-Blocking
- 비동기 방식의 논블로킹 방식은 '작업'에 대한 서로의 자유도가 높습니다. 각자 할일을 수행하며, 필요한 시점에 각자 결과를 처리합니다.

---

- **기다림** Blocking / **기다리지 않음** Non-blocking
- **내가 함** Synchronous / **다른 사람 시킴** Asynchronous

!https://blog.kakaocdn.net/dn/s4GhX/btry69JJgmj/LkjtlqXss4xSsMQUCnjx21/img.png

1. Sync-Blocking
    
    앞의 blocking 설명과 사실상 동일합니다. 내가 복합기 앞으로 갑니다. 시작 버튼을 누릅니다. 다른 일 못하고 기다립니다. 완료되면 그제야 내 자리로 돌아가 스캔 완료된 파일을 사용합니다.
    
2. Sync-NonBlocking
    
    앞의 non-blocking 설명과 동일합니다. 내가 복합기 시작 버튼을 누릅니다. 자리로 돌아옵니다. 틈틈이 스캔이 완료되었는지 확인해줘야 합니다. 하지만 다른 일을 할 수 있습니다. 다른 일을 못 하면서까지 앞에 가서 기다리지는 않습니다.
    
3. Async-Blocking (비효율)
    
    드디어 심부름꾼이 등장합니다. 나는 내 자리에 그대로 앉아있습니다. 나는 다른 일을 하는 동안, 심부름꾼에게 스캔을 하도록 시킵니다. 심부름꾼이 복합기 앞으로 갑니다. 시작 버튼을 누릅니다. 이 사람은 다른 일 못하고 이것만 기다립니다. 완료되면 심부름꾼은 내 자리로 돌아와 스캔이 완료되었다고 알려줍니다.
    
4. Async-NonBlocking
    
    나는 내 자리에 앉아서 내 일하는 동안, 심부름꾼에게 스캔을 하도록 시킵니다. 심부름꾼이 복합기 앞으로 가서 시작 버튼을 누릅니다. 심부름꾼은 스캔을 기다리면서 커피를 내려 마시면서 일기를 씁니다. 일기를 쓰는 동안, 틈틈이 스캔이 완료되었는지 곁눈질로 확인합니다. 스캔이 완료되면 내 자리로 돌아와 스캔이 완료되었다고 알려줍니다.



**참고**   
> - https://velog.io/@nittre/%EB%B8%94%EB%A1%9C%ED%82%B9-Vs.-%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EB%8F%99%EA%B8%B0-Vs.-%EB%B9%84%EB%8F%99%EA%B8%B0
> - https://choi-geonu.medium.com/%EB%B0%B1%EC%97%94%EB%93%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%93%A4%EC%9D%B4-%EC%95%8C%EC%95%84%EC%95%BC%ED%95%A0-%EB%8F%99%EC%8B%9C%EC%84%B1-2-%EB%B8%94%EB%A1%9C%ED%82%B9%EA%B3%BC-%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EB%8F%99%EA%B8%B0%EC%99%80-%EB%B9%84%EB%8F%99%EA%B8%B0-e11b3d01fdf8
> - [WebClient 개념과 사용법](https://gngsn.tistory.com/154)
> - [이미지 자료가 많은 블로그!](https://black7375.tistory.com/90#%EB%8F%99%EC%8B%9C%EC%84%B1-/-%EB%B3%91%EB%A0%AC)
> - [4 가지의 대한 예시!!!!!!!](https://github.com/NKLCWDT/cs/blob/main/Operating%20System/%EB%8F%99%EA%B8%B0_%EB%B9%84%EB%8F%99%EA%B8%B0_%EB%B8%94%EB%A1%9C%ED%82%B9_%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9.md)