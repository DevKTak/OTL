# **Layered Architecture to Hexagonal Architecture**

**레이어드 아키텍처**(**계층형 아키텍처**)로 되어있던 기존 프로젝트를 **헥사고날 아키텍처**(**포트 앤 어댑터 아키텍처**)로 리팩토링을 하였습니다. 먼저 각각에 대해서 간단하게 알아보겠습니다.  
<br>

## **Layered Architecture**
가장 일반적으로 많이 쓰이는 구조이며 보통 `프레젠테이션 -> 비즈니스 로직 -> 데이터 엑세스`의 3계층으로 구분을 하며 제어의 흐름은 상위에서 하위로 흐르고 이에 따른 소스코드의 의존성도 제어의 흐름을 따르게 됩니다.  
따라서 마지막에 있는 데이터 액세스 계층이 변경되었을 때, 비즈니스 로직 계층이 영향을 받게 됩니다.  
<br>

## **Hexagonal Architecture**
고수준의 비즈니스 로직(도메인 모델)을 표현하는 내부 영역과 저수준의 인터페이스 처리를 담당하는 외부 영역(인프라)으로 나뉩니다.  
헥사고날의 가장 큰 특징은 Dependency Rule이 적용되어 있는것입니다.

#### **Dependency Rule:** 
> 모든 소스코드 의존성은 반드시 Outer에서 Inner로, 고수준 정책을 향해야 한다.  

의존성은 반드시 밖에서 안으로만 존재해야하며, 내부 영역은 외부 영역에 대해서 알지(참조, 임포팅) 못하도록 해야합니다.  

```
Ports: 인터페이스, DI(Dependency Inversion) 를 위한 추상화, 변경이 잦은 어댑터와 애플리케이션의 결합도를 낮추는 역할

Adapters: 포트를 통해 인프라와 실제로 연결하는 부분만 담당하는 구현체 (인바운드 어댑터, 아웃바운드 어댑터)

Domain Model: 실제 핵심 비즈니스 로직을 처리하는 부분

Domain Service(클린 아키텍쳐에서의 UseCase): 도메인 모델과 어댑터를 이용해서 비즈니스 로직과 인프라를 오케스트레이션 하는 dumb한 레이어
```  
<br>

## **결론**
> 외부와의 통신을 인터페이스로 추상화하여 인프라의 변화가 생기더라도 외부 코드나 로직의 주입을 막아서 애플리케이션 동작(도메인 로직 혹은 비즈니스 로직)에는 영향을 주지 않는 구조인 헥사고날로 아래와 같이 리팩토링 해보았습니다. + 학습목적 

<br>

#### **예시) Payment 도메인**


<p align="center">
    <img src="https://github.com/DevKTak/OTL/assets/68748397/0b762379-5e32-47e9-80cd-252c1638295d" align="center" width="49%">
  <img src="https://github.com/DevKTak/OTL/assets/68748397/2c00fd8e-2fd8-450b-ae31-55719337227b" align="center" width="49%">
	<figcaption align="center" width="100%">레이어드 아키텍처(계층형 아키텍처)</figcaption>
</p>

***

<p align="center">
    <img src="https://github.com/DevKTak/OTL/assets/68748397/9fd26745-b046-4431-8732-cd6e8aea348e" align="center" width="49%">
  <img src="https://github.com/DevKTak/OTL/assets/68748397/d392c287-44f8-4537-aac6-19319e11ba91" align="center" width="49%">
	<figcaption align="center" width="100%">헥사고날 아키텍처(포트 앤 어댑터 아키텍처)</figcaption>
</p>
