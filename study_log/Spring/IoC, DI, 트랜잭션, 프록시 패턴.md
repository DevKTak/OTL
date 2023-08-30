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


# 프록시(Proxy)
> 프록시 객체는 원래 객체를 감싸고 있는 객체입니다. 대리자 역할을 합니다.
- Spring에서 사용하는 **`JDK Proxy(Dynamic Proxy)`** 와 **`CGLib`** 이 있습니다.
- SpringBoot에서는 기본적으로 **`JKD Proxy`** 를 사용합니다.

## 프록시 패턴을 사용하는 이유
- **보안 및 접근 제어:** 프록시를 사용하여 실제 객체에 접근하기 전에 인증 및 권한 검사를 수행할 수 있습니다.
- **지연 로딩(Lazy Loading):** 객체를 필요할 때 까지 생성하지 않고, 필요한 시점에 생성하는 것을 지연 로딩이라고 합니다. 프록시를 활용하여 실제 객체의 생성을 지연시키고, 실제 객체가 필요한 시점에 생성하도록 할 수 있습니다.
- **@Transactional:** 프록시를 사용하여 트랜잭션을 관리합니다.
- **AOP(Aspect-Oriented Programming):** 프록시를 사용하여 AOP를 구현하거나 관심사를 분리하여 보다 모듈화된 코드를 작성할 수 있습니다.

---

**참고**
> - [Spring의 트랜잭션 관리](https://yeonyeon.tistory.com/223)
> - [동일한 bean에서는 @Transactional 적용이 되지 않는다.](https://yeonyeon.tistory.com/283#nav)
> - [@Transactional 과 PROXY](https://velog.io/@chullll/Transactional-%EA%B3%BC-PROXY)
> - [Spring의 @Transactional과 AOP 그리고 CGLib와 Dynamic Proxy(JDK Proxy)](https://minkukjo.github.io/framework/2021/05/23/Spring/)