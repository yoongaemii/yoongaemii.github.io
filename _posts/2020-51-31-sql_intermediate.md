---
layout: post
title: "나 보려고 만든 SQL 요약 정리"
tags:
- sql
excerpt: "SQL은 평소에 쓸 일이 없어서 까먹기가 쉽다. 그래서 공부하면서 조금씩 정리한 요약 노트!"
---
> 코드카데미의 SQL: Table Transformation 코스와 [Mode라는 사이트](https://mode.com/sql-tutorial)의 intermediate sql 및 advanced sql 부분을 요약한 것이다.

## expressions
### CASE ... END
- 이 구문 자체가 하나의 새로운 칼럼이다. 그래서 `AS`로 이름을 지정해줘야 하는 것
- `WHEN`이 두 번 이상 나오면 두 번쨰 when 부터는 python의 `elif`와 같은 기능. 즉, 앞선 조건을 통과하지 않은 것에 대해서만 조건을 검사한다

### IFNULL(column, value)
- column의 값을 표시하되, null이면 value를 채워 넣는다
```sql
set @a := -1;
SELECT A.HOUR, IFNULL(B.COUNT,0) 
FROM (SELECT @a:=@a+1 as HOUR FROM ANIMAL_OUTS) A 
LEFT JOIN (SELECT HOUR(DATETIME) as HOUR, COUNT(*) as COUNT 
  FROM ANIMAL_OUTS 
  GROUP BY HOUR(DATETIME)) B
ON A.HOUR = B.HOUR 
WHERE A.HOUR<=23
```

## JOIN
### primary key
테이블 내에서 인스턴스를 구분할 수 있게 해주는 1) 값이 unique 하고 2) null 값을 가지지 않는 3) 단 하나의 칼럼
### foreign key
A column that contains the primary key of another table in the database. When the primary key for one table appears in a different table, it is called a foreign key.
- 관계를 맺고 있는 상대 엔터티의 인스턴스를 구분할 수 있게 하는 속성
- 대부분의 join은 한 테이블의 primary key를 다른 테이블의 foreign key와 매치한다

### JOIN 의 종류
1. inner join: [default] 두 테이블에서 조건을 만족하는 레코드만 반환
2. left (outer) join: 두 테이블에서 조건을 만족하는 레코드와 첫번째 테이블의 모든 레코드를 반환
3. right (outer) join: 두 테이블에서 조건을 만족하는 레코드와 두번째 테이블의 모든 레코드를 반환
4. full (outer) join: 두 테이블의 모든 레코드를 반환
> `COALESCE`: select의 칼럼 부분에서 파라미터로 주어진 칼럼들의 각 row를 하나씩 탐색하며 첫번째로 NULL이 아닌 값을 반환. FULL JOIN에서 on의 기준이된 칼럼을 선택하고자 할 때, 한 테이블의 칼럼으로는 join한 테이블의 모든 값을 포착하지 못할 수 있으므로 같이 쓰인다
```sql
SELECT COALESCE(A.year, B.year), SUM(A.volume), SUM(B.volume)
FROM A
FULL JOIN B
ON A.year = B.year
GROUP BY 1
```

### ON vs WHERE
- `ON` 이하의 조건은 JOIN이 일어나기 전에 평가되는 반면(key의 매칭을 판단하는 시점에) `WHERE`은 JOIN이 일어난 후에 조건을 심사한다
- 어떨 때 `ON` 을 활용한 filtering을 하는 건지 아직 잘 모르겠다. 어떤 글에서는 filtering은 오류 방지와 가독성을 위해서 where절로만 하라고 권고하기도

### CROSS JOIN
크로스 조인에서는 (칼럼명이 겹치지 않는다는 전제 하에) 칼럼의 출신 테이블을 생략한다
```sql
SELECT month, 
  COUNT(*)
FROM newspaper
CROSS JOIN months
WHERE start_month <= month 
  AND end_month >= month
GROUP BY month;
```

## SUBQUERY
### uncorrelated subqueries
- inner query가 outer query와 독립적으로 시행
- 다른 차원에서 두 번 집계해야할 때. 예를 들면 비행 로그 데이터에서 요일 별 하루 total distance의 평균을 구하려면 각 날짜 하루 동안의 total distance를 SUM으로 구한 뒤(inner subquery), 이를 요일 별로 AVG를 내야한다(outer subquery) \* 한 번에 요일 별로 group by를 하면 '하루 평균'이 안나온다!
### correlated subqueries
- inner query가 outer query에서 정의된 row에 대해서 여러번 시행(re-execute)되어야 할 때
- 항공사의 평균 운행 거리보다 멀리 운행한 flight를 집계하려면 flight의 항공사 별로 average를 다시 구해야한다

## WINDOW FUNCTION
- 기존의 aggregation function이 여러 개의 row를 합산하여 하나의 row(값)로 일축했다면, 윈도우 함수는 각 row들의 개별적인 정체성을 유지하면서도 전체 혹은 설정한 범위 안의 여러개의 row를 가로지르는 계산을 가능케 한다. 대표적인 예로는 옆에 누적 합을 표기하는 것.
- 여기서 '윈도우'란? 집계가 진행되는 데이터의 부분 집합 _the ordered subset of data over which calculations are made_
```sql
window_function (expression) OVER (
  PARTITION BY expr_list
  ORDER BY order_list [ frame_clause ] )  
```
### PARTITION
`PARTITION BY`가 없다면 전체에 대해서 윈도우 함수를 적용한다. partitioning을 하면 마치 group by 처럼 주어진 칼럼을 기준으로 그룹으로 나눈 뒤 집계한다
### ORDER
`ORDER BY`가 없다면 순서와 관계 없이 (전체 혹은 partition으로 주어진 범위의) 모든 row를 집계한다. order by가 있으면 주어진 칼럼을 기준으로 정렬하고, 해당 row와 그 이전에 등장한 row들의 값만 집계한다.
### window alias
쿼리 내에서 반복적으로 쓰이는 윈도우(over 뒤 partition이나 order에 의해 정의된 데이터의 부분집합)의 alias를 지정할 수 있다
```sql
SELECT start_terminal,
       duration_seconds,
       NTILE(4) OVER ntile_window AS quartile,
       NTILE(5) OVER ntile_window AS quintile,
       NTILE(100) OVER ntile_window AS percentile
  FROM A
WINDOW ntile_window AS
         (PARTITION BY start_terminal ORDER BY duration_seconds)
 ORDER BY start_terminal, duration_seconds
```

## Performance Tuning
1. table size: row의 개수를 줄이자!
- 쿼리를 돌리기 전에 `COUNT(*)`로 결과의 크기를 확인하고 계산 시간을 가늠해보자
- 데이터 탐색은 데이터의 부분 집합에 대해서 시행한 뒤에 전체 데이터로 확장하자
- 쿼리 결과가 아니라 로직을 확인하기 위해서라면 `LIMIT`를 적극적으로 활용하자. 단, aggregation function의 경우 데이터를 fetch하기 전에 무거운 계산이 시행되므로 limit는 소요 시간을 줄이지 못한다. 이 때는 subquery를 사용해서 데이터의 부분집합에 대해서만 aggregation을 시행하도록.
2. join: 조인은 row의 개수를 급격히 늘릴 수 있으므로 최대한 단순화하자
- 앞선 지침과 같은 원리로, join 전에 각 테이블의 크기를 최대한 줄인다. 예: join 후에 aggregation 하지 말고 aggregation 후에 join 하기
3. `EXPLAIN`을 활용하여 cost가 많이 드는 부분 확인하고 수정하자

## 앞으로
외국어를 처음 배울 때처럼 프로그래밍 언어도 처음은 다 설레고 신난다. 사실 앞으로가 문제다. SQL은 어떤 DBMS를 사용하냐에 따라서 지원하는 함수의 종류와 문법이 살짝씩 달라지다보니 어떻게 공부해야할지 고민이 된다. 주요한 개념은 훑었지만 문제풀이를 통해 datetime이나 string 등을 조작하는 다양한 함수를 익혀야 할 것 같은데 적당한 사이트를 아직 못 찾았다(프로그래머스는 SQL 문제가 몇 없더라). 