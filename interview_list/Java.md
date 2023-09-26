# JAVA

```
Q. Functional Interface
A. 
- 추상 메소드를 딱 하나만 가지고 있는 인터페이스
- static 메서드와 default 메서드의 개수에는 제약이 없다.
```

```
Q. String, StringBuilder, StringBuffer 장단점
A. 
- String: 불변성(immutable)이므로 동기화를 신경쓸 필요가 없어 안정성이 유지되는 장점, 문자열 추가, 수정, 삭제 등의 연산이 빈번하게 발생할 땐 힙 메모리(Heep)에 많은 임시 가비지(Garbage)가 생성되어 힙 메모리 부족으로 성능에 영향을 끼침.
- StringBuilder: 가변성(mutable),동기화를 지원하지 않기 때문에 단일쓰레드에서의 성능이 StringBuffer 보다 뛰어나다.
- StringBuffer: 가변성(mutable), 멀티쓰레드 환경에서 안전하다 (thread-safe).
```

```
Q. String, new String() 의 차이
A. 
- String: “Hello”라는 문자열은 JVM Heap 메모리의 String Pool로 저장하고 해당 참조값을 가짐 (String msg1 = "Hello";)
- new String(): Heap에 객체를 생성, new String(”Hello”).intern(); 으로 String pool에 등록할 수 있다. (String msg3 = new String("Hello");)
```

```
Q. equals(), hashcode()
A. 
- equals: 두 객체의 내용이 같은지, 동등성(equality) 를 비교하는 연산자
- hashcode: 두 객체가 같은 객체인지, 동일성(identity) 를 비교하는 연산자
- 같이 재정의 해야하는 이유: hashcode()를 재정의 하지 않으면 같은 값 객체라도 해시값이 다를 수 있다.
equals()를 재정의 하지 않으면 hascode()가 만든 해시값을 이용해 객체가 저장된 버킷을 찾을수는 있지만 해당 객체가 자신과 같은 객체인지 값을 비교할 수 없기 때문에 null을 리턴한다.
```
> 참조: [[Java] equals() & hashcode() 메서드는 언제 재정의해야 할까?](https://velog.io/@sonypark/Java-equals-hascode-%EB%A9%94%EC%84%9C%EB%93%9C%EB%8A%94-%EC%96%B8%EC%A0%9C-%EC%9E%AC%EC%A0%95%EC%9D%98%ED%95%B4%EC%95%BC-%ED%95%A0%EA%B9%8C)

<br>

```
Q. 제네릭이란?
A. JDK 1.5부터 도입된 제네릭을 사용하면 컴파일 시점에 미리 타입을 검사하여
    1. 클래스나 메소드 내부에서 사용되는 객체의 타입 안정성을 높일 수 있습니다.
    2. 반환값에 대한 타입 변환 및 타입 검사에 들어가는 노력을 줄일 수 있습니다.
```

```
Q. 리플렉션에 대해 설명하고 리플렉션의 장점을 살렸다고 생각하는 사례를 말하시오.
A. 구체적인 클래스 타입을 알지 못해도 그 클래스의 메서드, 타입, 변수들에 접근할 수 있도록 해주는 자바 API,
ComponentScan, DI, AOP, Data Binding,프로 퍼티 파일 로딩 및 설정(@Value)
```

```
Q. JVM 메모리 구조
A. 
- 스택: 지역 변수, 매개 변수, 쓰레드마다 런타임 스택을 만듬
- 힙: 인스턴스 변수(멤버변수), 객체, GC의 대상
- 메소드: (class 영역 == static 영역): 클래스 수준의 정보(클래스 데이터, 클래스 변수) 저장, 클래스가 로딩될 때 생성 ~ JVM이 종료될 때까지 유지
```

```
Q. JVM 동작 방식
A. 
1. 자바 컴파일러(javac)가 자바 소스코드(.java)를 자바 바이트코드(.class)로 컴파일한다.
2. Class Loader는 로딩, 링크, 초기화를 한 후 JVM 메모리에 올린다.
3. 실행엔진은 JVM 메모리에 올라온 바이트 코드들을 명령어 단위로 해석하여 실행한다.
```

```
Q. 자바 동기화처리
A. 여러 개의 쓰레드가 하나의 자원에 접근하려 할 때, 동시에 접근하는 것을 제한하여 순서를 지켜서 접근하도록 하는 것
```

```
Q. 직렬화(serialization)란?
A. 자바에서 입출력을 할 때에는 스트림이라는 통로를 통해 데이터가 이동합니다. 하지만 객체는 바이트형이 아니라서 스트림을 통해 파일에 저장하거나 네트워크로 전송할 수 없습니다.
따라서 객체는 스트림을 통해 입출력하려면 바이트 배열로 변환하는 것이 필요한데, 이를 '직렬화'라고 합니다. 반대로 스트림을 통해 받은 직렬화된 객체를 원래 모양으로 만드는 과정을 역직렬화라고 합니다.
직렬화: 자바 시스템 내부에서 사용되는 Object 또는 Data를 외부의 자바 시스템에서도 사용할 수 있도록 Byte 형태로 데이터를 변환하는 기술
역직렬화: Byte로 변환된 Data를 원래대로 Object나 Data로 변환하는 기술
```

```
Q. 코드 리팩토링의 의미가 무엇일까요?
A. 
- 결과의 변경 없이 코드의 구조를 재조정하는 것
- 가독성과 유지보수성을 높인다.
```

```
Q. Java 몇까지 써봤는지? 자바 버전별 새로생긴 부분 정리
A.
- 자바 8(LTS): 람다식, 스트림, java.time 패키지, Optional, 함수형 인터페이스, 인터페이스 디폴트 메서드
- 자바 11(LTS): String 메서드 추가
- 자바 14: Switch문 (표준화 됨), record
- 자바 17(LTS): Sealed Classes (표준화 됨)
- 자바 21(LTS): null에 해당하는 케이스를  스위치 내부에서 검사할 수 있음
```

<img width="784" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/9660cbb2-01f6-4efa-a5e2-fc6b6e7feea8">

<br>

```
Q. Java 8에서 Stream?
A. '파이프라인'과 같이 한줄에 연산처럼 풀어나가는 방식으로 해결하는 것
- 스트림 파이프라인 == 다수의 중개 오퍼레이션 + 한개의 종료 오퍼레이션
- 코드가 간결해진다.
```

```
Q. Enum 이란 무엇인가요?
A. 
- 서로 연관있는 상수들을 묶어놓은 집합
- final static을 이용해서 정의하던 문제점들을 보완할 수 있다.
- 가독성이 좋아 구현 의도 파악이 쉽다.
- 인스턴스 생성, 상속을 방지하여 사수의 타입안정성이 보장된다.
```

```
Q. 자바 reflection에 대해 설명해주세요
A. 
- 구체적인 클래스 타입을 알지 못해도 그 클래스의 메소드, 타입, 변수들에 접근할 수 있도록 해주는 자바 API
- 대표적인 사용 예: DI, Proxy, ModelMapper
```

```
Q. final
A.
- final class: 다른 클래스에서 상속하지 못합니다.
- final method: 다른 메소드에서 오버라이딩하지 못합니다.
- final variable: 변하지 않는 상수값이 되어 새로 할당할 수 없는 변수가 됩니다.
```

```
Q. 에러(Error)와 예외(Exception)의 차이점
A. 
- 에러(Error): 수습할 수 없는 심각한 문제
    - StackOverflowError: 브레이크없는 재귀 같은 경우
    - OutOfMemoryError: JVM이 할당한 메모리의 부족으로 더 이상 객체를 할당할 수 없을 때 던져지는 오류
- 예외(Exception): 개발자가 구현한 로직에서 발생한 실수나 사용자의 영향에 의해 발생
    - NullPointerException: 객체가 필요한 경우에 null을 사용하려고 시도할 경우 던져지는/던져질 수 있는 예외
    - IllegalArgumentException: 메서드가 허가되지 않거나 부적절한 argument를 받았을 경우에 던져지는/던져질 수 있는 예외
    - 예외는 오류와 다르게 개발자가 임의로 예외를 던질 수 있다.
```

```
Q. Checked Exception과 Unchecked Exception
A. 
- Checked Exception: 컴파일 시점에 확인되고 반드시 예외 처리를 해야하며 예외 발생 시 롤백하지 않음 (IOException, ClassNotFoundException 등)
- Unchecked Exception: 런타임 시점에 확인되고 예외처리를 명시적으로 하지 않아도 되며 예외 발생 시 롤백 해야 함 (NullPointerException, ClassCastException 등)
```

```
Q. 절차지향과 객체지향 차이
A. 
- 절차지향은 데이터를 중심으로 함수를 구현, 실행순서에 중점
- 객체지향은 기능을 중심으로 메소드를 구현
```

```
Q. 자바 8의 람다식(익명함수)이란
A. 
- 메서드를 하나의 식으로 표현한 것
- 메서드의 이름과 반환값이 없어지므로 익명 함수라고도 한다.
```

```
Q. Blocking/NonBlocking, Synchronous/Asynchronous
A. 
- Blocking/NonBlocking은 호출되는 함수가 바로 리턴하느냐 마느냐가 관심사다.
    - 호출된 함수가 바로 리턴해서 호출한 함수에게 제어권을 넘겨주고, 호출한 함수가 다른 일을 할 수 있는 기회를 줄 수 있으면 NonBlocking이다.
- Synchronous/Asynchronous는 호출되는 함수의 작업 완료 여부를 누가 신경쓰냐가 관심사다.
    - 호출하는 함수가 호출되는 함수의 작업 완료 후 리턴을 기다리거나 또는 호출되는 함수로부터 바로 리턴 받더라도 작업 완료 여부를 호출하는 함수 스스로 계속 확인하며 신경쓰면 Synchronous이다.
```

```
Q. Stream map, flatMap 차이
A. 
- map(): 단일 스트림 원소를 매핑 시킨 후 매핑 시킨 값을 다시 스트림으로 반환하는 중간 연산을 담당
- flatMap(): 여러 스트림을 한개의 스트림으로 합쳐줌
```
