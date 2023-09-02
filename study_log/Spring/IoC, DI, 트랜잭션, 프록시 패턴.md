# IoC(Inversion of Control)
> **프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것을 `제어의 역전`이라고 합니다.**

# DI(Dependency Injection)
> **DI는 의존성 주입을 의미하고 IoC 원칙을 실현하기 위한 디자인 패턴 중 하나이며, DI와 IoC 모두 객체간의 결합을 느슨하게 만들어 유연하고 확장성이 뛰어난 코드를 작성하기 위한 패턴입니다.**

# 트랜잭션
> **DB 상태를 변환시키는 `하나의 논리적 기능을 수행하기 위한 작업의 단위` 또는 한꺼번에 모두 수행되어야 할 일련의 연산들**

## 트랜잭션 ACID
- **Atomicity(원자성):** 트랜잭션 내의 작업들은 모두 성공 또는 모두 실패한다.
- **Consistency(일관성):** 모든 트랜잭션은 일관성 있는 DB 상태를 유지한다. (ex: DB의 무결성 제약 조건 항상 만족)
- **Isolation(격리성):** 동시에 실행되는 트랜잭션들은 서로 영향을 미치지 않는다. (ex: 동시에 같은 데이터 수정 X)
- **Durability(지속성):** 트랜잭션이 성공적으로 끝나면 그 결과는 항상 기록되어야 한다.

## 트랜잭션 격리 수준(Isolation Level)
- **READ UNCOMMITED:** 커밋되지 않은 데이터 읽기
- **READ COMMITED:** 커밋된 데이터만 읽기
- **REPEATABLE READ:** 한 트랜잭션 내에서 같은 읽기 보장
- **SERIALIZABLE:** 팬텀 리드 현상 개선

## @Transactional
- @Transactional은 메서드가 실행되기 전 begin을 호출하고, 메서드가 종료한 후 commit을 호출합니다.
- private 에서는 사용 불가

# 프록시(Proxy)
> **프록시 객체는 원래 객체를 감싸고 있는 가짜 객체입니다. 프록시 객체를 통해 타겟 객체에 접근하는 방식으로써 대리자 역할을 합니다.**
- Spring에서 사용하는 **`JDK Proxy(Dynamic Proxy)`** 와 **`CGLib`** 이 있습니다.
  
## JKD Proxy(Dynamic Proxy)
- JDK Proxy는 **`타겟의 인터페이스를 기준`** 으로 프록시 객체를 생성합니다.또한 JAVA의 **`reflection`** 을 사용하여 프록시를 생성합니다.
- **`Spring`** 에서는 기본적으로 **`JKD Proxy`** 를 사용합니다.

## CGLib
- 인터페이스를 구현하지 않고도 해당 구현체를 reflection 없이 **`타겟 클래스에 대한 바이트 코드 조작을 통해`** 객체를 생성해주는 라이브러리 입니다. 때문에 성능적으로 JDK Proxy 보다 좋습니다.
- **`SpringBoot`** 에서는 기본적으로 **`CGLib Proxy`** 를 사용합니다.

## 프록시 패턴을 사용하는 이유
- **보안 및 접근 제어:** 프록시를 사용하여 실제 객체에 접근하기 전에 인증 및 권한 검사를 수행할 수 있습니다.
- **지연 로딩(Lazy Loading):** 객체를 필요할 때 까지 생성하지 않고, 필요한 시점에 생성하는 것을 지연 로딩이라고 합니다. 프록시를 활용하여 실제 객체의 생성을 지연시키고, 실제 객체가 필요한 시점에 생성하도록 할 수 있습니다.
- **@Transactional:** 프록시를 사용하여 트랜잭션을 관리합니다.
- **AOP(Aspect-Oriented Programming):** 프록시를 사용하여 AOP를 구현하거나 관심사를 분리하여 보다 모듈화된 코드를 작성할 수 있습니다.

## Spring AOP는 왜 Target 객체를 직접 참조하지 않고 Proxy 방식을 사용할까?
Aspect 클래스에 정의된 부가 기능을 사용하기 위해서 원하는 위치에서 직접 Aspect 클래스를 호출해야 한다면 Target 클래스 안에 부가기능을 호출하는 로직이 포함되어야 합니다.
그렇게되면 여러곳에서 반복적으로 Aspect를 호출해야 하고 유지보구성이 크게 떨어집니다.
그래서 Spring에서는 Target 클래스 혹은 그의 상위 인터페이스를 상속하는 `프록시 클래스`를 생성하고 프록시 클래스에서 부가 기능에 관련된 처리를 하도록 합니다. 이렇게 해야 Target 클래스에서 Aspect를 알 필요없이 순수한 비즈니스 로직에 집중할 수 있습니다.

## Proxy 형태로 동작하는 @Transactional AOP

<img width="641" alt="image" src="https://github.com/FastCampusKDTBackend/KDT_Y_BE_Toy_Project1/assets/68748397/1fe89dd1-4c2e-4ec1-b19a-99e9e5e078d2">

1. target에 대한 호출이 들어오면 AOP Proxy가 이를 가로채서(Intercept) 가져옵니다.
2. AOP Proxy에서 **`Transaction Advisor`** 가 `commit` 또는 `rollback` 등의 트랜잭션 처리를 합니다.
3. 트랜잭션 처리 외에 다른 부가 기능이 있을 경우 해당 `Custom Advisor`에서 그 처리를 합니.
4. 각 Advisor에서 부가 기능 처리를 마치면 **`Target Method`** 를 수행합니다.
5. Interceptor chain을 따라 Caller에게 결과를 다시 전달합니다.
---

**참고**
> - [AOP와 @Transactional의 동작원리 !!!!](https://velog.io/@ann0905/AOP%EC%99%80-Transactional%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC)
> - [Spring의 트랜잭션 관리](https://yeonyeon.tistory.com/223)
> - [동일한 bean에서는 @Transactional 적용이 되지 않는다. -호돌- ](https://yeonyeon.tistory.com/283#nav)
> - [@Transactional 과 PROXY](https://velog.io/@chullll/Transactional-%EA%B3%BC-PROXY)
> - [Spring의 @Transactional과 AOP 그리고 CGLib와 Dynamic Proxy(JDK Proxy)](https://minkukjo.github.io/framework/2021/05/23/Spring/)