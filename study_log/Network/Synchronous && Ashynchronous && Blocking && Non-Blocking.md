
<img width="807" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/5730166e-d6ca-4c98-a8bb-765d9bbe8c72">

동기: 작업의 대한 순서가 보장되는 것

**`블로킹/논블로킹`이** **`제어권 반환`** 에 중점을 두었다면,
**`동기/비동기`는** **`결과를 바로 주는 지`** 에 초점을 맞추면 이해할 수 있습니다.

**`동기`는 작업 결과값을 직접 받아 처리하는 반면**, **`비동기`는 결과값을 받으면 어떻게 할지의** **`콜백함수`** 를 미리 정의해둡니다.

### Synchronous && Blocking
- 동기 방식의 블로킹은 수행한 대로 순서에 맞게 수행됩니다.
- ex) `read()`, `write()`

### Synchronous && Non-Blocking
- 동기 방식의 논블로킹은 작업을 시작하고 제어권을 다시 돌려주기 때문에 다른 작업을 수행할 수 있습니다.
- 종료 시점은 Application 딴의 Process 혹은 Thread가 Polling(지속적인 완료 확인)을 합니다.

### Asynchronous && Blocking
- 비효율적인 방식

### Asynchronous && Non-Blocking
- 비동기 방식의 논블로킹 방식은 '작업'에 대한 서로의 자유도가 높습니다. 각자 할일을 수행하며,  필요한 시점에 각자 결과를 처리합니다.

**참고**   
> - https://velog.io/@nittre/%EB%B8%94%EB%A1%9C%ED%82%B9-Vs.-%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EB%8F%99%EA%B8%B0-Vs.-%EB%B9%84%EB%8F%99%EA%B8%B0
> - https://choi-geonu.medium.com/%EB%B0%B1%EC%97%94%EB%93%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%93%A4%EC%9D%B4-%EC%95%8C%EC%95%84%EC%95%BC%ED%95%A0-%EB%8F%99%EC%8B%9C%EC%84%B1-2-%EB%B8%94%EB%A1%9C%ED%82%B9%EA%B3%BC-%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EB%8F%99%EA%B8%B0%EC%99%80-%EB%B9%84%EB%8F%99%EA%B8%B0-e11b3d01fdf8
> - [WebClient 개념과 사용법](https://gngsn.tistory.com/154)
> - [이미지 자료가 많은 블로그!](https://black7375.tistory.com/90#%EB%8F%99%EC%8B%9C%EC%84%B1-/-%EB%B3%91%EB%A0%AC)