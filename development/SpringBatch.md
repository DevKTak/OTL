# **Spring Batch**
새로운 기술을 프로젝트에 녹여내야 하는 상황이 생기면 저는 항상 당황~하지 않고 뙇! 유투브 또는 인프런 강의를 제일 먼저 찾아보는 성향이 있었습니다. 그러나 새로운 기술에 관련하여 유트브에 참고 할 만한 코드 영상이 없고, 인프런에도 없거나 또는 강의가 너무 비싸다 싶으면 이 순간 아까 하지 않았던 당황에 빠지고 맙니다.

앞으로 개발자로 살아가면서 이러한 상황의 연속일 것을 대비하여 이번 계기로 극복하는 연습을 하려고 합니다.

첫 번째 희생자 >>> ``Spring Batch``

해당 기술에 대해 전혀 무지한 상태에서 아래와 같은 단계로 적용해 보았습니다.
```
1. 블로그와 공식문서를 참조하여 예제 코드를 먼저 잘 실행되는지 부딪히고 봅니다.
2. 실행이 잘되면 처음보는 클래스나 메소드들을 검색하여 분석해봅니다.
3. 저의 프로젝트에 어떤식으로 녹여낼지 생각해보고 알맞게 커스텀합니다.
4. 코드를 깃허브나 블로그에 관련 지식과 개발 과정에서 실제 발생한 문제들을 기록하며 다시 한번 공부합니다.
```

## **1번, 2번**
---
> **1번과 2번의 단계는 아래 블로그와 공식문서를 참고 하였습니다.**
> 
> [https://jojoldu.tistory.com/category/Spring%20Batch](https://jojoldu.tistory.com/category/Spring%20Batch)<br>
> [https://docs.spring.io/spring-batch/docs/current/reference/html/index-single.html#whatsNew](https://docs.spring.io/spring-batch/docs/current/reference/html/index-single.html#whatsNew)

1번 같은 경우는 그냥 똑같이 따라하면 되겠지 싶은 부분인데 이상하게도 가장 오래걸리고 막히는 단계였습니다. <br>
이유는 여러 블로그마다 코드가 조금씩 다 달라서 매번 어느정도는 분석하느라 시간이 걸렸고, 스프링 배치의 빠른 변화로 @Deprecated 되거나 변경된 부분이 많았기 때문입니다.

저와 같은 상황인 spring-batch-core:5.0 이상 버전을 사용하시는 분들은 맨 하단에 해당 프로젝트를 참고해주세요.

## **3번**
---
다음으로 3번 단계에선 숙소의 룸마다 자정에 재고를 채워 넣어주는 기능을 구현하기 위해 스프링 배치를 프로젝트에 녹여내 보았습니다.

## **4번**
---
관련 지식과 개발 과정에서 실제 발생한 문제들을 기록해 보겠습니다. (@Deprecated 되거나 메소드의 파라미터 형식 변경 등의 문제는 해당 프로젝트의 코드를 참고 바라겠습니다.)

### **[관련 지식]**
**배치 애플리케이션이란?** `사람과 상호작용 없이` 이어지는 프로그램(작업)들의 실행 (웹 애플리케이션과 지향점이 다름)   
**스프링 배치란?** 대용량 데이터 일괄 처리를 위한 배치 프레임워크   
**스프링 배치를 사용하는 이유?** 
- 대용량 데이터 처리
- 일괄 처리
- 병렬 처리
- 재시도, 스킵, 건너뛰기 등의 오류 처리 전략, 트랜잭션 기능
- 사용자 개입없이 동작
- 모니터링

<br>
<img width="829" alt="배치 관련 객체 관계도" src="https://github.com/DevKTak/OTL/assets/ 68748397/f362e965-5459-456e-b498-71ce98954455">

**Job:** 배치 처리 작업의 `실행 단위`이며, Job은 여러 개의 Step으로 구성될 수 있습니다.
```java
  private final JobRepository jobRepository;
  private final JpaTransactionManager jpaTransactionManager;
  private final EntityManagerFactory entityManagerFactory;
  private final HotelJavaBatchConfigurationProperties properties;

  @Bean(name = "lastDayJob")
  public Job lastDayJob() {
    return new JobBuilder("lastDayJob", jobRepository)
        .preventRestart() // 재실행 막기
        .start(lastDayStep()) // Step을 인자로 전달하여 가장 기본이되는 SimpleJobBuilder를 생성합니다.
        .build();
  }
```
- JobRepository: 배치 처리 정보를 담고 있는 매커니즘, 어떤 Job이 실행되었으면 몇 번 실행되었고 언제 끝났는지 등 배치 처리에 대한 메타데이터를 저장
  
```java
public class JobBuilder extends JobBuilderHelper<JobBuilder> {

	@Deprecated(since = "5.0")
	public JobBuilder(String name) {
		super(name);
	}

	public JobBuilder(String name, JobRepository jobRepository) {
		super(name);
		super.repository(jobRepository);
	}

	public SimpleJobBuilder start(Step step) {
		return new SimpleJobBuilder(this).start(step);
	}

	public JobFlowBuilder start(Flow flow) {
		return new FlowJobBuilder(this).start(flow);
	}

	public JobFlowBuilder flow(Step step) {
		return new FlowJobBuilder(this).start(step);
	}
}
```
***
**Step:** Job을 구성하는 `작업의 단위`이며, 실질적인 배치 처리를 정의하고 제어하는데 필요한 모든 정보가 있는 도메인 객체입니다.
```java
  @Bean
  public Step lastDayStep() {
    return new StepBuilder("lastDayStep", jobRepository)
        //<Reader에서 반환할 타입, Writer에 파라미터로 넘어올 타입>
        // chunkSize: Reader & Writer가 묶일 Chunk 트랜잭션 범위
        // 쓰기 시에 청크 단위로 writer() 메서드를 실행시킬 단위를 지정, 즉 커밋단위가 properties.getStock().getChunkSize()
        .<Room, Stock>chunk(properties.getStock().getChunkSize(), jpaTransactionManager)
        .faultTolerant()
        .retryLimit(3)
        .retry(Exception.class) // Exception이 발생할 경우에만 최대 재시도 횟수 3회
        .reader(readerLastDay())
        .processor(processorLastDay(null))
        .writer(writerLastDay())
        .build();
  }
```
***
**ItemReader:** Step의 대상이 되는 배치 데이터를 읽어오는 인터페이스입니다. ItemReader의 가장 큰 장점은 데이터를 `Streaming` 할 수 있는 것입니다.
read() 메소드는 데이터를 하나씩 가져와 ItemWriter로 데이터를 전달하고, 다음 데이터를 다시 가져 옵니다.
이를 통해 **reader & processor & writer가 Chunk 단위로 수행되고 주기적으로 Commit 됩니다.**
수백, 수천 개 이상의 데이터를 한번에 가져와서 메모리에 올려놓게 되면 좋지 않기 때문에 배치 프로젝트에서 제공하는 PagingItemReader 구현체를 사용할 수 있습니다. 구현체들 중 저는 JpaPaginItemReader를 적용해보았습니다.
```java
  @StepScope
  @Bean(destroyMethod = "")
  public JpaPagingItemReader<Room> readerLastDay() {
    return new JpaPagingItemReaderBuilder<Room>()
        .name("readerLastDay")
        // JPA를 사용하기 때문에 영속성 관리를 위해 EntityManager를 할당해줘야 합니다.
        .entityManagerFactory(entityManagerFactory)
        .pageSize(properties.getStock().getChunkSize()) // pageSize 디폴트는 10
        .queryString("SELECT r FROM Room r WHERE r.stockBatchDateTime IS NOT NULL")
        .build();
  }
```
- chunkSize와 pageSize 갯수는 같게 해야합니다. 
- 예를들어 chunkSize = 100, pageSize = 10 이면 chunk 단위로 Reader에서 Processor로 전달되기 때문에 100개를 채워야만 데이터가 전달됩니다.   
10번을 조회해야해서 문제가 되는것이 아니라 JpaPagingItemReader는 페이지를 읽을 때, 이전 트랜잭션을 초기화 시키기 때문에 마지막 조회를 제외한 9번의 조회 결과들이 트랜잭션 세션이 전부 종료되어 **org.hibernate.LazyInitializationException**을 발생시킵니다.
***
**ItemProcesor:** ItemProcessor는 ItemReader로 읽어 온 배치 데이터를 변환하는 역할을 수행합니다. ItemProcessor는 필수로 만들 필요는 없습니다.   
비즈니스 로직의 분리 : ItemWriter는 저장 수행하고, ItemProcessor는 로직 처리만 수행해 역할을 명확하게 분리
     읽어온 배치 데이터와 씌여질 데이터의 타입이 다를 경우에 대응할 수 있기 때문
```java
  @Bean
  @StepScope
  public ItemProcessor<Room, Stock> processorLastDay(@Value("#{jobParameters[now]}") LocalDateTime now) {
    return room -> {
      room.changeStockBatchDateTime(now);

      return Stock.builder()
          .room(room)
          .date(LocalDate.now().plusDays(properties.getStock().getDay()))
          .quantity(properties.getStock().getQuantity())
          .build();
    };
  }
```
***
**ItemWriter:** 가공된 데이터를 저장하거나 다른 시스템으로 전송하는 역할을 담당합니다.   
Reader와 Processor를 거쳐 처리된 Item을 Chunk 단위 만큼 쌓은 뒤 이를 Writer에 전달합니다.   
Reader의 read()는 Item 하나를 반환하는 반면, Writer의 write()는 인자로 Item List를 받습니다.
```java
 private JpaItemWriter<Stock> writerLastDay() {
    JpaItemWriter<Stock> jpaItemWriter = new JpaItemWriter<>();
    jpaItemWriter.setEntityManagerFactory(entityManagerFactory);

    return jpaItemWriter;
  }
```
***
**@JobScope, @StepScope:** 보통의 빈 생성은 application 실행 시점에 환경변수를 모두 끌고와서 처음으로 Singleton 빈을 만들고 DI로 받아서 사용하는데  
반면, **`@JobScope`는 Step 선언문에서만 사용이 가능하고 Job이 실행될 때 Bean이 생성되고 `@StepScope`는 Step을 구성하는 ItemReader, ItemProcessor, ItemWriter에서 사용 가능하고 Step이 실행될 때 Bean이 생성됩니다.** `(애플리케이션 실행 시점이 아닌 Late Binding)`

**@JobParameter:** Spring Batch는 외부에서 파라미터를 주입받아 Batch 컴포넌트에서 사용할 수 있습니다.   
`[Step]에서 사용하려면 @JobScope`를 선언해줘야하고 `[Tasklet, Reader, Processor/ Writer] 에서 사용하려면 @StepScope`를 선언해줘야 합니다.   
사용법: `@Value("#{jobParameters[파라미터명]}") 타입 이름`
***
### **[개발 과정에서 실제 발생한 문제]**
<img width="982" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/a9793e70-168a-4636-9fb2-5327521cfadb">
<img width="982" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/44f04d90-d227-4106-a137-6df328122bc2">

Room과 Stock Entity는 1 : N 관계입니다.   
62라인에서 JPQL로 Room 객체를 영속성 컨텍스트에서 조회하고 Processor에 청크단위로 전달합니다.   
processorLastDay() 메소드에서 룸 객체에 있는 stockBatchDateTime 필드를 갱신하기 위해 70라인에서 chnageStockBatchDateTime() 메서드를 호출합니다.   
테스트 결과 room 테이블에 stockBatchDateTime 컬럼 값이 null 이였습니다.

저는 room 객체가 당연히 더티 체킹으로 stockBatchDateTime 컬럼에 현재 시간 값이 들어갈거라고 생각하였습니다.   
그러나 컬럼 값이 null 인것을 보고 영속 상태가 풀리는건가 싶어서 처음엔 아래 두 가지 방법으로 처리하였습니다.
```java
1. 
room.changeStockBatchDateTime(now);
Room mergedRoom = entityManager.merge(room);
```

```java
2. 
room.changeStockBatchDateTime(now);
Room persistRoom = roomRepository.findById(room.getId());
```

1. 가급적이면 EntityManager를 직접 사용하는 로직을 자제하는 편이 좋다고 생각되었습니다.   
2. 원하는대로 결과값은 나왔지만 아무리 생각해도 영속상태가 아닌게 믿기지 않았습니다.

### **결론**
> 관계 매핑이 된 컬럼에 대해서는 영속 상태 전이가 이뤄지지 않으면 더티 체킹이 되지 않는것 이였음으로 writer에서 Stock Entity가 저장될 때 영속 상태 전이 속성을 사용해서 Room Entity도 더티 체킹이 될 수 있도록 하였습니다.

```java
public class Stock extends BaseTimeEntity {
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  @ManyToOne(cascade = CascadeType.MERGE) // <-- 바로 이 부분
  @JoinColumn(name = "room_id")
  private Room room;
}
```
```java
private JpaItemWriter<Stock> writerLastDay() {
    JpaItemWriter<Stock> jpaItemWriter = new JpaItemWriter<>();
    jpaItemWriter.setEntityManagerFactory(entityManagerFactory);

    return jpaItemWriter;
  }
```
***

> 이렇게 처음 접하는 기술을 사용하는 것에 대한 두려움을 극복하는 첫 번째 훈련을 무사히 마쳤습니다.   
> **해당 프로젝트:** [https://github.com/f-lab-edu/hotel-java](https://github.com/f-lab-edu/hotel-java)