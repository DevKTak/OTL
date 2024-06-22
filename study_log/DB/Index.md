# Index
> - `정렬해놓은 컬럼 사본`, 보통 B tree 형태로 정렬해 놓습니다.   
> - 테이블의 레코드에 빠르게 접근하고 검색을 빠르게 수행하는데 도움을 줍니다.
> - 특정 열에 대한 검색 성능 향상 

## Index를 쓰는 이유?
**`조건을 만족하는 튜플(들)을 조회하기 위해!`**   
빠르게 `정렬(order by)`하거나 `그룹핑(group by)` 하기 위해!   
full scan: O(N), B-tree 기반 인덱스가 걸려있다면: O(logN)

1.인덱스를 통해 PK를 찾음   
2. PK를 통해 레코드를 찾음

```
이러한 이유로 옵티마이저는 인덱스를 통해 레코드 1건을 읽는 것이 테이블을 통해 직접 읽는 것 보다 4~5배 정도 비용이 더 많이 드는 것으로 예측한다.
하지만 DBMS는 우리가 원하는 레코드가 어디있는지 모르므로, 모든 테이블을 뒤져서 레코드를 찾아야한다. 이는 엄청난 디스크 읽기 작업이 필요하므로 상당히 느리다.
하지만 인덱스를 사용한다면 인덱스를 통해 PK를 찾고, PK를 통해 레코드를 저장된 위치에서 바로 가져올 수 있으므로 디스크 읽기가 줄어들게 된다.
그렇기 때문에 레코드를 찾는 속도가 훨씬 빠르며, 이것이 인덱스를 사용하는 이유이다.
반면에 인덱스를 타지 않는 것이 효율적일 수도 있다. 인덱스를 통해 레코드 1건을 읽는 것이 4~5배 정도 비싸기 때문에,
읽어야 할 레코드의 건수가 전체 테이블 레코드의 20~25%를 넘어서면 인덱스를 이용하지 않는 것이 효율적이다.
이런 경우 옵티마이저는 인덱스를 이용하지 않고 테이블 전체를 읽어서 처리한다.
```


## 인덱스의 단점
- 인덱스를 만들어 두는 만큼 하드 용량을 차지합니다.
- **원본 테이블에서 삽입, 수정, 삭제 같은 작업이 발생할 경우 index에도 똑같이 반영을 해줘야 하기 때문에 성능 하락이 있을 수 있습니다.**

## 보통 인덱스는 어떤 컬럼에 걸어 주나요?
- 주로 **`검색 조건으로 자주 사용되는 열`** 이나 **`JOIN 조건에 사용되는 열`** 에 인덱스를 생성하는 것이 좋습니다.

## 인덱스 관련 SQL문
```sql
[Mysql 기준] 

# 인덱스 거는 법
- CREATE INDEX 인덱스명 ON 테이블명(컬럼명)
- CREATE UNIQUE INDEX 인덱스명 ON 테이블명(컬럼명1, 컬럼명2) // 멀티 컬럼 인덱스(컬럼 2개 이상), 순서 중요, 앞에 컬럼 기준으로 먼저 정렬 후 뒤에 컬럼 정렬

# 모든 인덱스 확인
SHOW INDEX FROM 테이블명

# 어떤 인덱스를 사용하는지 알고 싶을 때
EXPLAIN SELECT * FROM ~~; // optimizer가 알아서 적절하게 index를 선택

# 옵티마이저가 이상하게 동작해서 직접 index를 고르고 싶다면?
SELECT * FROM 테이블명 USE INDEX(인덱스명) WHERE 조건 // 가급적 이 인덱스를 써주세요 느낌의 권장사항이고 강제로 무조건 이 인덱스를 써주세요 느낌으로 하려면?
SELECT * FROM 테이블명 FORCE INDEX(인덱스명) WHERE 조건

특정 인덱스를 제외하고싶을 때는?
SELECT * FROM 테이블명 IGNORE INDEX(인덱스명) WHERE 조건
```

## Covering index
<img width="1257" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/dc8dc792-dbe1-46c2-b06d-28e7d71b8621">

# B tree
> 데이터를 저장하고 검색하기 위한 `자체 균형 트리` 구조입니다.

## B tree 특징
![image](https://github.com/DevKTak/DevKTak/assets/68748397/ec32f613-602b-4c18-9d4f-ff480fc241c0)
- **`모든 리프 노드들이 같은 레벨`** 을 가지며 **`정렬된 순서를 보장`** 합니다.
- **`자녀 노드의 최대 개수를 늘리기 위해서 부모 노드에 key를 하나 이상 저장합니다.`**
- 부모 노드의 key들을 오름차순으로 정렬합니다.
- 정렬된 순서에 따라 대소 비교를 하여 자녀 노드 들의 key 값의 범위도 결정합니다.
- **최대 M개의 자녀를 가질 수 있는 B tree**를 **`M차 B tree`** 라 부릅니다.
- (B tree, B+ tree, B* tree): 삽입, 삭제, 조회의 avg case, worst case 모두 시간 복잡도 **`O(logN)`**

## B tree 기반의 index가 동작하는 방식
<img width="691" alt="image" src="https://github.com/DevKTak/DevKTak/assets/68748397/3f4ea399-23ac-43f0-b907-ce925c034988">

1. 바이너리 서치를 통해 찾으려는 where 절 a 컬럼의 인덱스에서 찾으려는 값을 찾은 후 포인터를 사용해서 실제 테이블에 튜플을 찾아갑니다.
2. 중복 값이 있을 수도 있으니 남은 바이너리 서치를 합니다.

## B tree 데이터 조회
- **`루트노드에서 시작하여 하향식으로 검색을 수행합니다.`**

## B tree 데이터 삽입
- **추가는 항상 leaf 노드**에 합니다.
- 노드가 넘치면 가운데(median) key를 기준으로 좌우 key들은 분할하고 가운데 key는 승진합니다.

## B tree 데이터 삭제
- **삭제도 항상 leaf 노드**에서 합니다.
- 삭제 후 최소 key 수보다 적어졌다면 재조정합니다.

## B tree 계열을 DB 인덱스(index)로 사용하는 이유
![image](https://github.com/DevKTak/DevKTak/assets/68748397/597dc927-94b4-470e-9846-990c048dfe0e)
- DB는 기본적으로 secondary storage에 저장됩니다.
- DB에서 데이터를 조회할 때 secondary storage에 최대한 적게 접근하는 것이 성능 면에서 좋습니다.
- secondary storage는 block 단위로 읽고 씁니다.
- **B tree는 노드에 여러개의 데이터를 저장**할 수 있기 때문에 **block 단위 저장 공간**을 알차게 사용할 수 있습니다.
- self-balancing BST에 비해 **secondary storage 접근을 적게** 합니다.

## AVL 트리를 인덱스로 사용하여 `where b = 5` 를 찾는 예시
<img width="1341" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/5af3e9da-e040-4188-9633-6387771f2f47">

## B 트리를 인덱스로 사용하여 `where b = 5` 를 찾는 예시
<img width="1391" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/45e019be-3d4a-462d-bda0-10bcb4c241af">

## AVL 트리 vs B 트리 결과 비교 
<img width="1427" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/9cdec462-d691-4950-8121-a07d20067240">

<img width="1433" alt="image" src="https://github.com/DevKTak/OTL/assets/68748397/b138d5d4-fb30-41cf-acc3-48a257d09760">

## Hash index를 쓰는건?
- hash index는 삽입/삭제/조회의 시간 복잡도가 O(1)
- mysql은 hash index를 제공하기도 합니다.
- 하지만 **`=, <, > 와 같은 범위 기반의 검색이나 정렬에는 사용될 수 없고`**, **`equality(=) 값 비교 조회만 가능`** 하다는 단점이 있습니다.
- 해시 인덱스는 해시 테이블로 구현되어 있기 때문에 array를 활용해서 저장이 되는데 데이터가 쌓이다 보면 늘려줘야하는 순간에 더 큰 사이즈로 바꿔주는것을 rehashing이라고 하고 **`리해싱에 대한 부담`** 이 있습니다.

# B+tree
> `B-tree의 변형`으로 `리프 노드에만 데이터가 저장되며` `내부 노드는 키만 가집니다.` 검색 및 범위 검색에 사용됩니다.

<img width="1461" alt="image" src="https://github.com/DevKTak/DevKTak/assets/68748397/33578bc6-f269-4ee6-90e2-17ef3785a8a8">

# B*tree
> 주로 메모리 기반 데이터베이스에서 사용됩니다.

### 참고
> - [DB index](https://www.youtube.com/watch?v=IMDH4iAQ6zM)   
> - [B tree의 개념과 특징](https://www.youtube.com/watch?v=bqkcoSm_rCs)

# 추가
## B tree에서 삽입 시 비용이 비싼 이유
루트 노드부터 다시 탐색을 해야하기 때문에

## index(a, b) 멀티 컬럼 인덱스로 설정하고 where b = 1 and a = 3 순서로 조회하면 인덱스를 탈까?
앞에 컬럼 기준으로 먼저 정렬 후 뒤에 컬럼 정렬하기 때문에 인덱스를 제대로 타지 못합니다. 순서가 중요합니다. 결국 root부터 leaf 노드까지 탐색하게되는 인덱스 풀스캔이 일어납니다.
