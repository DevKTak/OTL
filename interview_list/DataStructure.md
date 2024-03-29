# Data Structure
![img](https://github.com/f-lab-edu/hotel-java/assets/68748397/45b19457-1508-4bfe-af65-568b8c0e1ba6)

```
Q. Collection
A. 다수의 데이터를 쉽고 효과적으로 처리할 수 있는 표준화된 방법을 제공하는 클래스의 집합
- Thread Safe한 컬렉션: Vector, HashTable, SynchronizedList, SynchronizedMap, SynchronizedSet, ConcurrentHashMap, ConcurrentQueue
- Thread Safe 하지 않은 컬렉션: ArrayList, HashSet, HashMap
```

```
Q. 해쉬맵에 대해서 설명해주세요. 
A. 어떤 키에 대해서 값을 저장하는 자료구조체이며 해쉬 함수를 이용하여 키-값을 저장합니다. 이때 해쉬 함수는 출력 값이 제한되어 있기 때문에 해쉬 충돌이 일어나는데 이를 해결하기 위한 제일 쉬운 방법은 각 해쉬 배열에 링크드 리스트를 연결한 체이닝 방식이 있습니다.
```

## Hash Collision(해시 충돌)
> 키 값이 다른데, 해시 함수의 결과값이 동일한 경우

## 해시충돌 해결 방법
## **1. Chaining**

<img width="50%" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/9c3dc7f3-ae1f-4c0f-8a75-b2186b45535e">   

- `Chaining` 방식은 해시 함수 결과값이 같을 경우 연결 리스트로 체인처럼 연결하는 방식이기 때문에 O(N)의 시간복잡도를 가질 수 있습니다.
- 최근에는 자바에서 리스트 대신 트리를 사용하여 시간복잡도를 조금 단축하였습니다.

## **2. Open Addressing**
> 충돌 발생시 다른 버킷에 데이터를 저장

<img width="50%" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/145edd8d-54e6-4a6c-b61d-ac40540d00af">

- 해시 충돌 시 n칸만 건너뛴 버킷에 저장하면되기때문에 계산은 단순하나 검색 시 10번 인덱스의 값이 있기 때문에 11번으로 인덱스로 건너뛰고 11번에 넣으려했으나 또 11번 버킷에 데이터가 있는 상황이면 또 이동해야해서 이런식으로 검색하다보면 시간복잡도가 O(N)이 될 수 있습니다.
- 더 큰 문제는 데이터들이 특정 위치에만 밀집(clustering) 되는것 입니다. 좋은 해시펑션은 키를 고르게 분포 시키는 것 입니다. 밀집 될 수록 충돌로 데이터의 위치를 재탐색하면 곧 성능의 저하를 가져오기 때문입니다.

---

<img width="50%" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/acd0f3d2-9f7f-4622-8ec9-4bbd95fa10f4">

- 선형 탐색의 밀집하는 문제를 해결하기 위해 나왔으나 처음 해시 값 충돌이 일어난다면 결국 같은 위치에 밀집되는 문제가 발생

---

<img width="50%" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/cdc62138-0111-40e0-b84f-72f7dce51f77">

- 충돌 발생시 이동 폭을 구하는 해시펑션을 사용
- 클러스터링 문제 해결

---
## HashSet
- 중복된 데이터 저장 X
- 해시맵처럼 키밸류는 아니지만 마찬가지로 key를 저장한다. 
- 해시펑션을 통해서 인덱스의 위치를 찾아서 저장한다.

---

```
Q. Array vs LinkedList vs ArrayList
A. 
- Array: 연속된 메모리 공간에 존재
    - 장점: 순차적으로 데이터를 저장하기 때문에 index로 특정 요소 접근 가능
    - 단점: 정적 크기
- LinkedList: 각 노드가 데이터와 포인터를 가지고 있는 선형 데이터 자료 구조
    - 장점: 동적 크기, 배열의 복사나 재할당없이 데이터 추가 가능
    - 단점: 각 요소마다 포인터의 메모리 공간이 필요함, 포인터를 찾아가는 시간이 필요하기 때문에 배열에 비해 접근속도가 느리다., 삽입/삭제를 하려면 첫번째 원소부터 다 확인해봐야한다.
    - 추가/삭제: O(1), ArrayList처럼 밀거나 당겨줄 필요없이 포인터만 옮겨주면 되지만 current node 포인터가 해당 index까지 도달해야만 추가/삭제를 할 수 있기 때문에 전체적인 추가/삭제의 시간복잡도는 O(N)으로 볼 수 있습니다.
    - 검색: O(N)
- ArrayList: 연속된 메모리 공간에 존재
    - 장점: 동적 크기
    - 단점: 삽입/삭제 하는 경우에 복사가 일어나 성능 저하를 일으킬 수 있다.
    - 추가: O(1), 배열이 꽉 차있는 경우에 배열의 사이즈를 재할당하고 데이터를 옮기는 시간이 있기때문에 이런 경우엔 O(N) 
    - 추가/삭제: O(N), 데이터를 줄줄이 뒤로 밀어거나 당겨야 하기 때문, 추가적인 메모리 할당시 메모리 낭비
    - 검색: O(1)

* 조회를 많이 사용하면 배열 리스트, 쓰기를 많이 사용하면 연결 리스트를 선택
```
![image](https://github.com/f-lab-edu/hotel-java/assets/68748397/8a725964-5ae3-4388-ab7a-02ed51105a99)

<img width="858" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/312dc466-b2bd-4fbb-8e18-3fce7f66227f">

```
Q. ArrayList 내부적으로 어떻게 구현되어 있을까? 배열로 구현되어있다면 크기가 꽉차면 일반 배열처럼 예외가 발생할텐데 어떻게 무한히 데이터를 받을 수 있을까?
A. 첫 element를 add 할 때 배열의 resize가 발생하고 배열 크기는 10으로 설정된다. 이후 가지고 있던 용량이 꽉 찼을때 현재 용량의 1.5배(기존의 크기 + 기존의 크기 / 2)를 늘린 새로운 배열에 기존 배열을 copy한다.
```

```
Q. HashTable vs CouncurrentHashMap
A. 
HashTable: 
- 모든 메소드에 동기화 되어 있습니다. 즉, 한번에 하나의 스레드만 HashTable 메소드를 사용할 수 있습니다. 고로 멀티스레드 환경에서 성능이 떨어집니다.
- 키와 값에 null을 허용하지 않습니다.

CouncurrentHashMap
- 세분화된 잠금(lock splitting) 및 분할 잠금(split locks) 등의 기술을 사용하여 세분화된 동기화를 제공합니다. 이는 동시에 여러 스레드에서 안전하게 읽고 쓸 수 있도록 합니다.
- 키와 값에 null을 허용합니다.
```

```
Q. Java에서 HashMap 충돌을 관리하는 방법
A.
1. 버킷 사이즈 조절: HashMap이 처음 생성되면 버킷사이즈는 16이고 16의 75%가 임계점이므로 threshold는 12가 된다. 만약 버킷에 데이터가 저장되어 그 수가 12를 넘어가면 버킷의 사이즈는 원래 사이즈의 2배인 32가 된다. 그럼 threahold도 2배 커진 24가 된다.
2. Linked List O(N) + Red Black Tree O(logN): 충돌 초기에는 separate chaining 방식으로 Linked List를 이용하여 충돌에 대응한다. 지정된 임계점이 넣으면 Red Black Tree 방식으로 객체들을 저장한다.
```

```
Q. List vs Set vs Map
A.
- List는 기본적으로 데이터들이 순서대로 저장되며 중복을 허용한다.
- Set은 순서가 보장되지 않고 데이터들의 중복을 허용하지 않는다.
- Map은 순서가 보장되지 않고 Key 값의 중복은 허용하지 않지만 Value 값의 중복은 허용된다.
```

```
Q. TreeMap vs HashMap
A.
같은 Tree 구조로 이루어진 TreeSet과의 차이점은 TreeSet은 그냥 값만 저장한다면 TreeMap은 키와 값 형태로 저장한다.
TreeMap은 저장과 동시에 키가 자동 오름차순 정렬되며 숫자타입일 경우에는 값으로, 문자열 타입일 경우에는 유니코드로 정렬한다.
TreeMap은 HashMap 보다 성능은 떨어지나 정렬된 데이터를 조회해야하는 범위 검색이 필요한 경우 효율적이다.
TreeMap은 이진탐색트리의 문제점을 보완한 레드-블랙 트리로 이루어져있다.   
```

```
Q. LinkedHashMap이란  
A: HashMap의 모든 기능은 그대로 사용할 수 있다.
추가적으로, 저장된 데이터들의 순서를 유지한다.
순서를 유지하기 위해 이중 연결 리스트 (Doubly Linked List)를 사용한다. (HashMap은 자료구조로 배열을 사용한다.)
따라서 메모리 사용량이 HashMap 보다 높다.
```

```
BST(Binary Search Tree)의 문제점:

BST의 입력 순서에 따라 트리가 불균형해질 수 있습니다. 즉, 트리의 한 쪽 가지가 다른 쪽에 비해 훨씬 더 깊게 나가는 현상이 발생할 수 있습니다. 이로 인해 탐색 시간이 효율적이지 않을 수 있습니다.
Balanced Search Tree(레드-블랙 트리, AVL 트리 등)는 균형을 유지하며, 어떤 노드도 다른 노드에 비해 더 많은 레벨을 가지지 않습니다. 이로 인해 최악의 경우에도 탐색 시간이 O(log n)으로 유지됩니다.
```
