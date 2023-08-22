# Index
> 정렬해놓은 컬럼 사본, 보통 B tree 형태로 정렬해 놓습니다.

## Index를 쓰는 이유?
조건을 만족하는 튜플(들)을 조회하기 위해!   
빠르게 `정렬(order by)`하거나 `그룹핑(group by)` 하기 위해!

## 인덱스의 단점
- 인덱스를 만들어 두는 만큼 하드 용량을 차지합니다.
- 원본 테이블에서 삽입, 수정, 삭제 같은 작업이 발생할 경우 index에도 똑같이 반영을 해줘야 하기 때문에 성능 하락이 있을 수 있습니다.

# B tree
## B tree 특징
![image](https://github.com/DevKTak/DevKTak/assets/68748397/ec32f613-602b-4c18-9d4f-ff480fc241c0)
- 자녀 노드의 최대 개수를 늘리기 위해서 부모 노드에 key를 하나 이상 저장합니다.
- 부모 노드의 key들을 오름차순으로 정렬합니다.
- 정렬된 순서에 따라 대소 비교를 하여 자녀 노드 들의 key 값의 범위도 결정합니다.
- **최대 M개의 자녀를 가질 수 있는 B tree**를 **`M차 B tree`** 라 부릅니다.
- (B tree, B+ tree, B* tree): 삽입, 삭제, 조회의 avg case, worst case 모두 시간 복잡도 O(logN)

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
- 하지만 =, <, > 와 같은 범위 기반의 검색이나 정렬에는 사용될 수 없고, **equality(=) 값 비교 조회만 가능**하다는 단점이 있습니다.

# B+tree

### 참고
> - [DB index](https://www.youtube.com/watch?v=IMDH4iAQ6zM)   
> - [B tree의 개념과 특징](https://www.youtube.com/watch?v=bqkcoSm_rCs)
