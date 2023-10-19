# JPA N + 1 문제가 발생하였습니다!🙏
# N + 1 문제란?
> **`1번의 쿼리를 날렸을 때`** **`의도하지 않은 N번의 쿼리`** 가 추가적으로 실행되는 것을 의미합니다.

## ✅ 연관관계 애노테이션 별 default 로딩
### 즉시 로딩(FetchType.EAGER)
- @ManyToOne
- @OneToOne
### 지연 로딩(FetchType.LAZY)
- @OneToMany
- @ManyToMany

## ✅ N + 1 문제가 발생한 상황
### ➲ Entity 간의 관계   
<img width="861" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/83343ce9-9b7f-4e71-9a70-d170f8066639">

#### **Accommodation(숙소) Entity**
<img width="599" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/3b731e90-3940-4414-adda-c0aa3790ffa2">

#### **Room(객실) Entity**
<img width="530" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/a2f347c9-c325-4e99-a902-32d711840c59">

#### **Stock(재고) Entity**
<img width="374" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/13465e3a-0a42-40f3-bca2-a5cf1a454e6f">

---

### ➲ Accommodation(숙소)와 Room(객실) 테이블의 데이터
#### **Accommodation(숙소) 테이블**
<img width="2198" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/9d05519c-b732-4c04-badb-610ea73c60a4">

#### **Room(객실) 테이블**
<img width="2059" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/dada825c-7663-4440-b192-16c2edccee83">

---

### ➲ 쿼리
<img width="1077" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/39bf9b08-a268-46b3-baec-de234b4e8663">

---

### ➲ 실제 호출 코드
<img width="1184" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/c3a11178-f720-4a83-895a-970715118fde">

> **51 라인: 지연 로딩으로 인해 프록시 객체로 가지고 있던 Room 데이터를 필요한 시점이 되어 데이터베이스에 로딩하며 N개의 쿼리가 추가로 발생합니다.**

---

### ➲ N + 1 쿼리 로그
<img width="586" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/b758320a-6fde-428f-a020-d19edfa95e33">   

> **N + 1 중 1에 해당하는 엔티티 조회 쿼리(1번)가 나갔고,**

<div style="display: flex; justify-content: space-between;">
    <img width="49%" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/82bfd616-891a-4d2b-8ea2-508f6618f91f">
    <img width="49%" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/97fb504d-28a6-44e3-a7ae-39e1a67676c1">
</div>

> **N에 해당하는 조회된 엔티티의 개수(N개) 만큼 연관된 엔티티 쿼리 2번이 나갔습니다.**   
> **현재 테이블에 존재하는 데이터의 개수만큼 쿼리가 2 + 1번이 실행 되었습니다.**   
> **만약 Accommodation 테이블에 숙소가 1000개 있었다면, 숙소 한번 조회하려다가 1001번의 쿼리가 나가는 것입니다.**

## ✅ 문제 해결 
### ➲ N + 1 문제를 해결하기 위한 시도 1
<img width="1077" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/0cec0490-7192-435b-9fb2-bba9e8483719">

<img width="2088" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/f5385648-5e9b-4cae-87b4-8d3229b58263">

> N + 1 문제는 당연하듯이? 단순하게? "**`Fetch Join`** 쓰면 되지" 라고 생각 하였습니다.   
> **`MutipleBagFetchException`** 에게 당했습니다.   
> 
> **결론: Fetch Join은 단일 관계의 자식 테이블에 사용하여야 하고, 2개 이상의 OneToMay 자식 테이블에 Fetch Join을 선언하게되면 위와 같은 예외가 발생합니다.**

### ➲ N + 1 문제를 해결하기 위한 시도 2
<img width="603" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/5a62f491-441a-4179-ac57-eee0200bdadd">

<img width="374" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/8d90574e-491c-421a-ac8c-7a7b008a0878">

Fetch Join 처럼 한방 쿼리는 아니지만 Hibernate의 배치 사이즈 옵션을 사용하였습니다.   

**참고: 테스트 코드 전용 애노테이션**
```
@TestPropertySource(properties = "spring.jpa.properties.hibernate.default_batch_fetch_size=100")
```

위에 [N + 1 쿼리 로그] 캡처 사진에서 보셨다싶이
```sql
select * from room where accommodation_id = 1;   
select * from room where accommodation_id = 2;
```
위와 같이 쿼리가 두 번이 수행 되었었는데, 배치 사이즈 옵션을 사용하면 아래와 같이 한 번으로 최적화 할 수 있습니다.
```sql
select * from room where  accommodation_id in (1, 2);
```   
**참고**   
in절 파라미터로 1,000개 이상을 주었을 때 너무 많은 in절 파라미터로 인해 문제가 발생할 수 도 있기 때문에 보통 옵션값으로 1,000 이상을 주지 않는다고 합니다.   
Accommodation(숙소)가 1,000개 까지는 1 + 1 = 2 번의 쿼리로 끝낼 수 있겠군요!

## ✅ N + 1 문제 결론
batch fetch가 활성화되었을 때 Hibernate는 많은 쿼리를 준비하게 되고, 이 과정에서 많은 메모리를 잡아먹게 됩니다. 따라서 전역적으로 적용하는 batch_size는 10, 50 같은 작은 숫자로 적용하고, 커다란 batch 작업이 필요할 때에는 @BatchSize를 이용해 큰 값을 설정해주는 것이 이상적이라고 합니다.

## ✅ 해결 과정에서 추가로 생긴 의문점
<img width="917" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/142eeb81-9cbf-423e-8386-36fae27cec20">

> 로그에서 실제로 실행된 쿼리를 DBMS 툴인 데이터그립에서 실행 해보았을 때   
> accommodation 테이블에 숙소 2개 * room 테이블에 룸 3개 = 총 6개의 데이터가 나왔습니다.   
> 그러나 코드상에서 리턴된 값의 size가 2 였고 distinct를 넣은것도 아니여서 한동안 멘붕에 빠졌습니다.. 

**결론**   
하이버네이트 6 이후부터는 자동으로 **`distinct`** 를 해준다고 합니다.!

*** 
> **해당 프로젝트:** [https://github.com/f-lab-edu/hotel-java](https://github.com/f-lab-edu/hotel-java)