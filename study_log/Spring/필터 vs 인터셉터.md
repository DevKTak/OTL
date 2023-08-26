# 필터 vs 인터셉터
## 서블릿 필터 vs 스프링 인터셉터
<img src="https://github.com/f-lab-edu/hotel-java/assets/68748397/23b89718-b229-4f61-9bce-f902ce2b21ca" width="85%">
<img width="85%" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/08c3673d-3f80-4477-9c58-5bab54bb7f48">

### 공통점
> 웹과 관련된 공통 관심사항을 처리한다는 공통점을 가지고 있습니다.

### 차이점
> 참고: [필터(Filter)가 스프링 빈 등록과 주입이 가능한 이유(DelegatingFilterProxy의 등장](https://mangkyu.tistory.com/221)

### 필터의 사용사례
> 스프링과 무관하게 전역적으로 처리해야 하는 작업들을 처리
- 보안 및 인증/인가 관련 작업 (XSS 방어)
- 모든 요청에 대한 로깅 또는 검사
- 이미지/데이터 압축 및 문자열 인코딩
- Spring과 분리되어야 하는 기능

### 인터셉터의 사용사례
> 클라이언트의 요청과 관련되어 전역적으로 처리해야 하는 작업들을 처리
- 세부적인 보안 및 인증/인가 공통 작업 (admin 권한 체크)
- API 호출에 대한 로깅 또는 검사
- Controller로 넘겨주는 정보(데이터)의 가공