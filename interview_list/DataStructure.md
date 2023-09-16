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

```
Q. Array vs LinkedList vs ArrayList
A. 
- Array: 연속된 메모리 공간에 존재
    - 장점: 순차적으로 데이터를 저장하기 때문에 index로 특정 요소 접근 가능
    - 단점: 정적 크기
- LinkedList: 각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 방식으로 데이터를 저장하는 자료 구조
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
```
![image](https://github.com/f-lab-edu/hotel-java/assets/68748397/8a725964-5ae3-4388-ab7a-02ed51105a99)

<img width="858" alt="image" src="https://github.com/f-lab-edu/hotel-java/assets/68748397/312dc466-b2bd-4fbb-8e18-3fce7f66227f">

```
Q. ArrayList 내부적으로 어떻게 구현되어 있을까? 배열로 구현되어있다면 크기가 꽉차면 일반 배열처럼 예외가 발생할텐데 어떻게 무한히 데이터를 받을 수 있을까?
A. 첫 element를 add 할 때 배열의 resize가 발생하고 배열 크기는 10으로 설정된다. 이후 가지고 있던 용량이 꽉 찼을때 현재 용량의 1.5배(기존의 크기 + 기존의 크기 / 2)를 늘린 새로운 배열에 기존 배열을 copy한다.
```

```
Q. List vs Set vs Map
A.
- List는 기본적으로 데이터들이 순서대로 저장되며 중복을 허용한다.
- Set은 순서가 보장되지 않고 데이터들의 중복을 허용하지 않는다.
- Map은 순서가 보장되지 않고 Key 값의 중복은 허용하지 않지만 Value 값의 중복은 허용된다.
```
