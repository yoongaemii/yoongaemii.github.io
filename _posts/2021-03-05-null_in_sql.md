---
layout: post
title: "까다로운 NULL(널)값 SQL로 두드려보고 건너자"
tags:
    - SQL
    - Database
excerpt: "DISTINCT는 null 값까지 셀까? GROUP BY를 하면 null값을 가진 행들도 하나의 그룹으로 묶일까? 동일한 뷰에 대해서 COUNT(*)를 실행한 결과와 특정한 칼럼에 대해 COUNT() 함수를 실행할 결과는 왜 달라지는 걸까?"
---
<br>    
<br>
- DISTINCT는 null 값까지 셀까? ⭕️
- GROUP BY를 하면 null값을 가진 행들도 하나의 그룹으로 묶일까? ⭕️
- 동일한 뷰에 대해서 COUNT(*)를 실행한 결과와 특정한 칼럼에 대해 COUNT() 함수를 실행할 결과는 왜 달라지는 걸까? -> COUNT(*)은 전체 행의 개수를, COUNT(칼럼명)은 해당 칼럼에서 null값을 제외한 행의 개수를 세기 때문

-------

위와 같은 null의 동작 사례를 들으면 등골이 조금 오싹해진다. 간단한 문제들이지만 무심코 지나치 큰 오류로 번질 수 있기 때문이다. 

실제 데이터를 담고 있는 테이블을 다룰 때에는 항상 null이 있을 수 있다. 원래 데이터가 입력되지 않은 경우도 있지만 join 등 쿼리작업으로 null이 생기기도 한다. 그만큼 실수의 위험도 도사리고 있다는 뜻이고...🥲 __null을 섬세하게 다루는 것은 예상치 못한 오류를 방지하는 첫걸음!__ 까다로운 null을 처리하는 방법과 실제 용례를 살펴봤다.

## NULL이란
null이란 모르는 값이다. __모르는 값이므로 다른 값과 비교 자체가 불가능하다.__
- NULL과의 수치연산은 NULL을 리턴한다(모르는값에 3을 더해도 모르는 값이다)
- NULL과의 비교연산(null > 3 등)은 FALSE를 리턴한다(비교 자체가 성립하지 않는다)

그래도 실제로 null을 포함하는 테이블을 쓰게 될 때는 사용하는 데이터베이스에서 연산의 결과를 한 번 확인해보는 것이 좋다. 데이터베이스에 따라 null을 취급하는 방식이 다른 경우도 있기 때문이다. Oracle에서는 NULL을 가장 큰 값으로 간주하여 오름차순으로 정렬했을 경우 가장 마지막에 배치하는 반면, SQL Server는 그 반대다. 그러니까 __돌다리도 두드려 보고 건너자!!__

돌다리를 하나하나 두드려 보며 확인하는 대신 여러 함수로 null로 인한 여러 상황에 대비할 수 있다.

| Null 처리함수 |      내용      
|-------------|--------------|
|`NVL(expresssion1, expression2)`   | expression1 의 결과값이 null이면 expression2의 값을 출력한다.|
|`NULLIF(expression1, expression2)` | expression1이 expression2와 같으면 __null__ 을, 같지 않으면 __expression1__을 출력한다.|
|`COALESCE(*expressions)`           | 임의의 개수의 표현식에서 null이 아닌 최초의 표현식을 나타낸다. 모두 null이라면 null을 출력하다. |

## NULL이 있는 칼럼의 집계
`SUM`, `AVG` 등 집계함수는 대상 칼럼에서 null을 가진 행을 제외하고 수행한다. 즉, null은 어차피 연산의 대상이 아니므로 불필요하게 NULL을 대체하는 0을 넣지 않아도 된다.
- 불필요한 변환 예시: `SUM(ISNULL(SAL, 0))`
- 최종 출력시 NULL이 아닌 0을 표시하고 싶은 경우(상황에 따라 필요): `ISNULL(SUM(SAL), 0)`

글머리에 제기한 질문에서 `COUNT(*)`과 `COUNT(단일칼럼)`의 결과가 다를 __수__ 있는 이유도 이 때문이다. COUNT(*)는 모든 칼럼이 집계의 대상이며, 대상이 되는 행들 중 모든 칼럼이 null인 경우 집계에서 제외한다. COUNT(단일칼럼)은 집계 대상이 되는 그 칼럼만 null인 경우 집계에서 제외한다.

 user_id | register_date | register_device
|----------|-------------:|------:|
 U001    | 2016-08-26    |               1
 U002    | 2016-08-26    |               2
 U001    | 2016-08-26    |               1
 U002    | 2016-08-26    |               2
 U003    | 2016-08-27    |
 U003    | 2016-08-27    |

```sql
SELECT COUNT(*) AS count_all,
       COUNT(register_device) AS count_device
FROM mst_users;
```
```sql
-- 모든 칼럼을 대상으로 한 집계와 단일 칼럼을 대상으로 한 집계가 다르다
 count_all | count_device
-----------+--------------
         6 |            4
```


## NULL vs 공집합
null과 가장 헷갈리는 것이 `공집합`이다. 공집합은 '조건에 맞는 데이터가 한 건도 없는 경우'를 말한다. 

어떻게 null과 헷갈리냐고? __쿼리 결과로 null이 나왔을 때와 공집합이 나왔을 때를 구분하기가 힘들기 때문이다.__ 단일 쿼리로 데이터베이스와 직접 대화하는 환경이라면, 화면 상에서 '데이터를 찾을 수 없다'라는 등의 문구나 row의 개수로 공집합을 구분할 수 있지만 다른 개발 언어 내에 문장이 포함된 경우에는 NULL과 공집합을 쉽게 구분하기 힘들다.

여러 쿼리가 연쇄적으로 들어가는 환경에서 둘 을 확실하게 구분하기 위해 __공집합을 빈 칸이 아닌 9999 등 의미없는 값으로 채우는 방법__ 이 자주 쓰인다. 앞서 언급한 것처럼 집계함수는 결과값이 공집합인 경우에도 NULL을 출력하므로 NVL/ISNULL 함수를 활용하여 dummy value로 대체하는 것이다.
```sql
SELECT MGR FROM EMP WHERE ENAME='JSC'; -- 공집합
SELECT NVL(MGR, 9999) MGR FROM EMP WHERE ENAME='JSC'; -- 잘못된 쿼리; 공집합에는 NVL 적용 불가
SELECT MAX(MGR) MGR FROM EMP WHERE ENAME='JSC'; -- NULL 출력
SELECT NVL(MAX(MGR), 9999) MGR FROM EMP WHERE ENAME='JSC'; -- 9999 출력
```
