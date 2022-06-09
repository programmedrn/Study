Postgresql 재귀 쿼리
====================

형태
----

```
WITH RECURSIVE ViewName(çcustom_column1, custom_column2, ...) AS (
    초기 쿼리
  UNITON [ALL]
    재귀 쿼리 WHERE 재귀의 수행 조건
)
ViewName을 활용한 쿼리
```

주의점
------

재귀를 멈추는 조건을 고민해야 함

자주 쓰이는 트릭
----------------

-	View에 Depth 칼럼 추가

초기 값을 1 또는 0으로 두고 tree 구조상 level을 표현하기 위함

```
 ex)
 WITH RECURSIVE t(n, depth) AS (
    SELECT 1, 0
   UNION ALL
    SELECT n*2, depth+1 FROM t WHERE depth < 10
   )
  SELECT n, depth FROM t WHERE depth = 5
// n  |  depth
// 32 |    5
2^5를 계산하는 쿼리
```

-	PATH와 CYCLE 칼럼

graph 구조에선 계층형이 아니라 다시 초기 값이 재귀문에서 반환될 수 있다. 이 경우에 반환된 초기 값을 수행하지 않기 위해 아래와 같은 방법을 사용

```
 ex)
 WITH RECURSIVE t(n, depth, path, cycle) AS (
    SELECT 1, 0, ARRAY[1], false
   UNION ALL
    SELECT n*2, depth+1, ARRAY_APPEND(super.path, n*2), n*2=any(super.path)
    FROM t AS super
    WHERE depth < 10
      AND NOT cycle
   )
  SELECT n, depth, path, cycle FROM t WHERE depth = 5

* ARRAY_APPEND는 super.path||n*2로 대체할 수 있다.
* 배열은 int, text만 담을 수 있다.
* = any는 배열에 해당 값이 포함돼있으면 true
path 칼럼에 array로 path를 두고 해당 row를 리턴하기까지 경로에 포함된 칼럼은 재귀문에서 제외하기 위함(순환 방지)
```
