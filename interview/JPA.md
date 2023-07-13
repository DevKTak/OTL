# JPA
> **Q. 영속성 라이프 사이클**   
> A. 비영속(transient), 영속(managed), 준영속(detached), 삭제(removed)

> **Q. save, merge, persist**   
> A.  
> merge: 병합은 준영속, 비영속을 신경 쓰지 않는다. 식별자 값으로 엔티티를 조회할 수 있으면 불러서 병합하고 조회할 수 없으면 새로 생성해서 병합한다. 따라서 병합은 save or update 기능을 수행한다. (자바 ORM 표준 JPA 프로그래밍 P.119), 영속 상태면 Update, 비영속, 준영속 상태면 Insert   
>
> persist: insert 쿼리만 실행
>
> save: 이 메소드는 저장할 엔티티가 `새로운 엔티티면 저장(persist)`하고 `이미 있는 엔티티면 병합(merge)`한다. 새로운 엔티티를 판단하는 기본 전략은 엔티티의 식별자로 판단하는데 식별자가 객체일 때 null, 자바 기본 타입일 때 숫자 0 값이면 새로운 엔티티로 판단한다. (자바 ORM 표준 JPA 프로그래밍 P.563)

> **Q. JPA와 Mybatis 장단점**   
> A.
> ORM: Object-Relational Mapping의 약어로 객체와 관계형 데이터베이스를 Mapping 시켜주는 것을 말한다. 
> JPA: Java Persistence API의 약자로 Java ORM 기술에 대한 API 표준 명세이다.
> - 장점: 1차 캐시, 쓰기지연, 더티체킹, 지연로딩, 컴파일 시점에 오류 확인, DB에 종속적이지 않음
> Mybatis: Java 클래스 코드와 SQL 코드를 Mapping 시켜준다.
> - 장점: SQL 쿼리를 직접 작성하므로 최적화된 쿼리를 구현할 수 있다.

> **Q. Gradle과 Maven의 차이**   
> A.   
> Gradle: Groovy(Java 가상머신에서 실행되는 스크립트 언어) 사용, Maven보다 최대 100배 빠름(캐시를 사용하기 때문에 테스트 반복 시 차이가 더 커짐), 가독성이 더 좋음   
> Maven: XML 사용

> **Q. 영속성 컨텍스트, EntityManagerFactory, EntityManager**   
> A.   
> **영속성 컨텍스트:** 영속성 컨텍스트를 관리하는 모든 엔티티 매니저가 초기화 및 종료되지 않는 한 엔티티를 영구히 저장하는 환경   
> **EntityManagerFactory:** 공장(엔티티 매니저 팩토리)에서 제품(엔티티 매니저)를 찍어내는 개념, 여러 스레드가 동시에 접근해도 안전하다.   
> **EntityManager:** 여러 스레드가 동시에 접근하면 동시성 문제가 발생하므로 스레드 간에 절대 공유하면 안된다. 상황에 따라서 계속해서 만들어줘야한다.

> **Q. 마이바티스 # 과 $ 차이**   
> A.   
> #{}: 쿼리에 작은 따옴표가 자동으로 붙어 실행된다. sql문이 아니라 단순 문자열로 해석된다.   
> \${}: ‘${id}’ 이런식으로 있으면 id의 파라미터 값으로 `'OR 1-1 --`, `admin’ --` 이 입력되면 뒷부분은 주석처리되므로 where 문의 조건을 항상 참이 되도록 만든다. 이와 같이 SQL Injection에 취약하다.   
> 참조: [#{}과 ${} 차이](https://madplay.github.io/post/difference-between-dollar-sign-and-sharp-sign-in-mybatis)
