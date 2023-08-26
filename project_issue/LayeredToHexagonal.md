# **Layered Architecture to Hexagonal Architecture**

**레이어드 아키텍처**(**계층형 아키텍처**)로 되어있던 기존 프로젝트를 **헥사고날 아키텍처**(**포트 앤 어댑터 아키텍처**)로 리팩토링을 하였습니다. 먼저 각각에 대해서 간단하게 알아보겠습니다.  
<br>

## **Layered Architecture**
가장 일반적으로 많이 쓰이는 구조이며 보통 `프레젠테이션 -> 비즈니스 로직 -> 데이터 엑세스`의 3계층으로 구분을 하며 제어의 흐름은 상위에서 하위로 흐르고 이에 따른 소스코드의 의존성도 제어의 흐름을 따르게 됩니다.  
따라서 마지막에 있는 데이터 액세스 계층이 변경되었을 때, 비즈니스 로직 계층이 영향을 받게 됩니다.  

Spring의 경우     
Presentation Layer -> View와 Controller,   
Business Layer -> Service   
Persistence Layer -> DAO   
Database Layer -> 데이터베이스 부분   
이렇게 계층 아키텍쳐를 구현하는 방법 중 하나로 MVC 패턴의 Model, View, Controller가 사용됩니다.

<div style="display: flex">
  <img width="50%" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/c31b7d52-1aaa-4a60-bd65-11f44c6dbc59">

 <img width="50%" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/c0f1ca77-6bec-41da-a528-a5c6c47ba316">
</div>

<div style="display: flex">
  <img width="50%" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/b93b496c-83b5-46e0-a1d9-4c0c6c324a3e">

  <img width="50%" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/54f4bf2a-b801-4da6-8d7e-b52ce32a741d">
</div>

## **Hexagonal Architecture**
고수준의 비즈니스 로직(도메인 모델)을 표현하는 내부 영역과 저수준의 인터페이스 처리를 담당하는 외부 영역(인프라)으로 나뉩니다.  
**헥사고날의 가장 큰 특징은 `Dependency Rule`이 적용되어 있는것입니다.**

#### **Dependency Rule:** 
> 모든 소스코드 의존성은 반드시 Outer에서 Inner로, 고수준 정책을 향해야 한다.  

의존성은 반드시 밖에서 안으로만 존재해야하며, 내부 영역은 외부 영역에 대해서 알지(참조, 임포팅) 못하도록 해야합니다.  

#### **Dependency Rule 적용으로 인한 장점 예시 코드**   
```java
Domain Model (도메인 모델):

// Order.java
public class Order {
    private long orderId;
    private String customerName;
    private List<OrderItem> items;

    // Getters and setters
}

// OrderItem.java
public class OrderItem {
    private long itemId;
    private String productName;
    private int quantity;

    // Getters and setters
}

Persistence Layer (퍼시스턴스 레이어):

// OrderRepository.java (인터페이스)
public interface OrderRepository {
    Order findById(long orderId);
    List<Order> findAll();
    void save(Order order);
    void update(Order order);
    void delete(long orderId);
}

// InMemoryOrderRepository.java (인메모리 데이터베이스를 사용하는 구현체)
public class InMemoryOrderRepository implements OrderRepository {
    private Map<Long, Order> orderData = new HashMap<>();

    @Override
    public Order findById(long orderId) {
        return orderData.get(orderId);
    }

    @Override
    public List<Order> findAll() {
        return new ArrayList<>(orderData.values());
    }

    @Override
    public void save(Order order) {
        orderData.put(order.getOrderId(), order);
    }

    @Override
    public void update(Order order) {
        orderData.put(order.getOrderId(), order);
    }

    @Override
    public void delete(long orderId) {
        orderData.remove(orderId);
    }
}

Service Layer (서비스 레이어):

// OrderService.java
public class OrderService {
    private OrderRepository orderRepository;

    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    public Order createOrder(String customerName, List<OrderItem> items) {
        long nextOrderId = generateNextOrderId();
        Order order = new Order(nextOrderId, customerName, items);
        orderRepository.save(order);
        return order;
    }

    public Order findOrderById(long orderId) {
        return orderRepository.findById(orderId);
    }

    public List<Order> getAllOrders() {
        return orderRepository.findAll();
    }

    // 기타 주문과 관련된 비즈니스 로직들
    // ...
}
```
위 코드에서 OrderService는 OrderRepository 인터페이스에만 의존합니다. 실제 데이터 액세스 계층의 구현체(InMemoryOrderRepository)는 의존성 주입을 통해 외부에서 주입되므로, OrderService 클래스는 데이터 액세스 계층의 구현체를 알 필요가 없습니다.

만약 데이터베이스 연동 방식이 변경되어 새로운 데이터베이스 연동 방식을 사용하려면, InMemoryOrderRepository를 새로운 데이터베이스 연동 방식에 맞게 수정하고 새로운 구현체(JdbcOrderRepository 또는 HibernateOrderRepository 등)를 작성합니다. 그러나 OrderService 클래스는 수정할 필요가 없이 그대로 사용할 수 있습니다. 이는 OrderService 클래스가 OrderRepository 인터페이스에만 의존하기 때문입니다.

이렇게 헥사고날 아키텍처에서는 데이터 액세스 계층의 변경이 비즈니스 로직 계층에 영향을 덜 미치도록 설계되어 있습니다. 비즈니스 로직 계층과 데이터 액세스 계층을 완전히 분리하고, 인터페이스를 사용하여 의존성을 역전시킴으로써 더 유연하고 확장 가능한 아키텍처를 구현할 수 있습니다.

```
Ports: 인터페이스, DI(Dependency Inversion) 를 위한 추상화, 변경이 잦은 어댑터와 애플리케이션의 결합도를 낮추는 역할

Adapters: 포트를 통해 인프라와 실제로 연결하는 부분만 담당하는 구현체 (인바운드 어댑터, 아웃바운드 어댑터)

Domain Model: 실제 핵심 비즈니스 로직을 처리하는 부분

Domain Service(클린 아키텍쳐에서의 UseCase): 도메인 모델과 어댑터를 이용해서 비즈니스 로직과 인프라를 오케스트레이션 하는 dumb한 레이어
```  

<img width="60%" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/08ee9312-2cb7-4b11-b8ed-a15dea749e81">

### 헥사고날 아키텍처의 장점
--- 
1. 명확한 관심사의 분리
   - 외부와의 연결에 문제가 생기면? 어댑터
   - 인터페이스는? 포트
   - 처리중간에 EventBridge에 이벤트를 보내거나 트레이스 로그를 심고 싶다면? 서비스
   - 비즈니스 로직이 제대로 동작하지 않으면? 도메인 모델
2. 쉬운 테스트
   - 자기 역할만 Port 기반 모킹을 통해 테스트
   - **비즈니스 로직은 의존성이 없기 때문에 모킹(mocking)이 거의 없음**

## **결론**
> 외부와의 통신을 인터페이스로 추상화하여 인프라의 변화가 생기더라도 외부 코드나 로직의 주입을 막아서 애플리케이션 동작(도메인 로직 혹은 비즈니스 로직)에는 영향을 주지 않는 구조인 헥사고날로 아래와 같이 리팩토링 해보았습니다. + 학습목적
> 
> 참고: 코딩량이 많아지고 패키지 구조에 대한 혼란함을 가중시키지 않을까 하는 우려의 조언을 듣고 in/out을 따로 만들지 않았습니다.

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

*** 
> **해당 프로젝트:** [https://github.com/f-lab-edu/hotel-java](https://github.com/f-lab-edu/hotel-java)
