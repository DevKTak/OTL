# Index
> - `정렬해놓은 컬럼 사본`, 보통 B tree 형태로 정렬해 놓습니다.   
> - 테이블의 레코드에 빠르게 접근하고 검색을 빠르게 수행하는데 도움을 줍니다.
> - 특정 열에 대한 검색 성능 향상 

## Index를 쓰는 이유?
조건을 만족하는 튜플(들)을 조회하기 위해!   
빠르게 `정렬(order by)`하거나 `그룹핑(group by)` 하기 위해!

## 인덱스의 단점
- 인덱스를 만들어 두는 만큼 하드 용량을 차지합니다.
- 원본 테이블에서 삽입, 수정, 삭제 같은 작업이 발생할 경우 index에도 똑같이 반영을 해줘야 하기 때문에 성능 하락이 있을 수 있습니다.

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

# B tree
> 데이터를 저장하고 검색하기 위한 `자체 균형 트리` 구조입니다.

## B tree 특징
![image](https://github.com/DevKTak/DevKTak/assets/68748397/ec32f613-602b-4c18-9d4f-ff480fc241c0)
- **`모든 리프 노드들이 같은 레벨`** 을 가지며 **`정렬된 순서를 보장`** 합니다.
- 자녀 노드의 최대 개수를 늘리기 위해서 부모 노드에 key를 하나 이상 저장합니다.
- 부모 노드의 key들을 오름차순으로 정렬합니다.
- 정렬된 순서에 따라 대소 비교를 하여 자녀 노드 들의 key 값의 범위도 결정합니다.
- **최대 M개의 자녀를 가질 수 있는 B tree**를 **`M차 B tree`** 라 부릅니다.
- (B tree, B+ tree, B* tree): 삽입, 삭제, 조회의 avg case, worst case 모두 시간 복잡도 O(logN)

## B tree 기반의 index가 동작하는 방식
<img width="691" alt="image" src="https://github.com/DevKTak/DevKTak/assets/68748397/3f4ea399-23ac-43f0-b907-ce925c034988">

- 바이너리 서치를 통해 찾으려는 where 절 a 컬럼의 인덱스에서 찾으려는 값을 찾은 후 포인터를 사용해서 실제 테이블에 튜플을 찾아갑니다.
- 중복 값이 있을 수도 있으니 남은 바이너리 서치를 합니다.

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
- self-balancing BST에 비해 **secondary strage 접근을 적게** 합니다.

## Hash index를 쓰는건?
- hash index는 삽입/삭제/조회의 시간 복잡도가 O(1)
- mysql은 hash index를 제공하기도 합니다.
- 하지만 **`=, <, > 와 같은 범위 기반의 검색이나 정렬에는 사용될 수 없고`**, **`equality(=) 값 비교 조회만 가능`** 하다는 단점이 있습니다.
- 해시 인덱스는 해시 테이블로 구현되어 있기 때문에 array를 활용해서 저장이 되는데 데이터가 쌓이다 보면 늘려줘야하는 순간에 더 큰 사이즈로 바꿔주는것을 rehashing이라고 하고 **`리해싱에 대한 부담`** 이 있습니다.

# B+tree
> B-tree의 변형으로 리프 노드에만 데이터가 저장되며 내부 노드는 키만 가집니다. 검색 및 범위 검색에 사용됩니다.
<img width="1461" alt="image" src="https://github.com/DevKTak/DevKTak/assets/68748397/33578bc6-f269-4ee6-90e2-17ef3785a8a8">

# B*tree
> 주로 메모리 기반 데이터베이스에서 사용됩니다.

### 참고
> - [DB index](https://www.youtube.com/watch?v=IMDH4iAQ6zM)   
> - [B tree의 개념과 특징](https://www.youtube.com/watch?v=bqkcoSm_rCs)
