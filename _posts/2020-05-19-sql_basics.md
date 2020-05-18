---
layout: post
title: "설계 원리를 궁예하며 익히는 SQL 기본 문법"
tags:
- sql
excerpt: "SQL은 관계형 데이터베이스를 다루는 데에 특화된 언어라 그런지 일반적인 프로그래밍 언어를 배울 때와는 많이 다른 인상을 받았다. 코드카데미에서 제공하는 learn sql 코스를 완주하며 들었던 궁금증을 스스로 해결하며 sql의 언어적 특성을 몇 가지로 정리해 보았다."
---

코드카데미는 sql의 기본 문법(maniqulation, query, aggregate functions, multiple tables)를 배울 수 있는 [인터랙티브 코스](https://www.codecademy.com/learn/learn-sql)를 무료로 제공한다. sql 코드는 항상 읽기만 했던지라 손에 익힌다는 느낌으로 기본 문법을 복습해봤다. 그 과정에서 파이썬이나 자바 등의 프로그래밍 언어와는 확실히 다르다는 느낌을 받아서 '설계 원리'라는 거창한 이름으로 정리해봤다. 그냥 나의 관찰과 느낌에 기반한 궁예질이라 틀릴 수도 있다. 특히 기본문법은 대부분 단일 테이블에서 데이터를 다루는 명령어들에 치중되어 있기 때문에 더 익히다 보면 다른 느낌이 들지도 모르겠다. 어쨌든 거시적인 틀을 잡아야 백지 상태에서 코드를 짜 나갈 수 있으니까. 코드카데미를 풀면서 들었던 궁금증도 곁다리로 정리했다.

## SQL은 테이블의 칼럼에 대한 언어이다
관계형 데이터 베이스는 여러 개의 테이블로 구성되고, 테이블의 기본은 칼럼이다. 기본 명령어들은 모두 칼럼을 생성하고 조회하고 삭제하는 작업을 위해 만들어졌다고 해도 과언이 아니다. 그리고 이 칼럼의 자리에 무엇이 들어갈 수 있는가를 확장해나가는 식으로 다른 명령어들이 파생된다. 파이썬의 `if ... else`문에 해당하는 `CASE ... END` 명령어의 예시를 보자
```sql
SELECT name,
  CASE
    WHEN review > 4.5 THEN 'Extraordinary'
    WHEN review > 4 THEN 'Excellent'
    WHEN review > 3 THEN 'Good'
    WHEN review > 2 THEN 'Fair'
    ELSE 'Poor'
  END
FROM nomnom;
```
위 예시는 레스토랑의 평점 데이터 nomnom에 대해 리뷰 점수를 구간에 따라 'Extraordinary'부터 'Poor'까지로 이름을 붙여서 조회한다. `CASE ... END` 문법은 평이하니 __어디에 들어갔는지__ 를 살펴보자. SELECT의 파라미터 자리, 즉 칼럼명 부분에 들어갔다. 'case 문은 조건에 맞게 데이터를 변환한다'라고 설명하면 반쪽짜리 설명이다. 그렇게 변환한 데이터를 새로운 칼럼으로 만드는 것까지가 case 문의 역할이고 이는 독립적이고 온전한 하나의 칼럼이니 select 문으로 선택하여 조회할 수 있다.

> `WHEN`이 두 번 이상 나오면 두 번쨰 when 부터는 python의 `elif`와 같은 기능을 한다. 즉, 앞선 조건을 통과하지 않은 것에 대해서만 조건을 검사한다. 두 번쨰 when의 조건을 `review <= 4.5 AND review > 4`로 명시하지 않아도 된다는 뜻이다. 눈치가 빠른 언어라는 느낌...  

이는 aggregation function도 마찬가지다. aggregation function을 시행한 결과는 모두 칼럼 파라미터 자리에 들어간다. 

## 칼럼을 간단히 부르는 방법이 발달했다
앞서 `CASE ... END` 문도 하나의 온전한 칼럼임을 살펴본 바 있다. 위 예시 코드를 그대로 실행하면 case 부터 어쩌구 저쩌구 end까지 모두 칼럼의 이름으로 들어가는 끔찍한 결과가 이를 말해준다. 그래서 `AS` 가 필요한 것이다. 스키마에 정의된 테이블 내 기존의 칼럼 뿐 아니라 데이터를 목적에 맞게 처리해서 호출한 함수도 칼럼이기 때문에 필연적으로 칼럼 이름이 길어질 수 밖에 없었고, 그래서 `AS`처럼 칼럼을 간단히 reference하는 방법이 발달했다.

<p align="center"  style="font-size:80%;">
  <img src="{{site.baseurl}}/images/sql_basics_withoutas.png" alt="sql without as" width="80%"/>
  AS가 없을 때의 끔찍한 sql 세계. 믿기지 않겠지만 짤린 저 문자열이 모두 칼럼 이름이다
</p>  

<p align="center"  style="font-size:80%;">
  <img src="{{site.baseurl}}/images/sql_basics_As.png" alt="sql with as" width="80%"/>
  (개비스콘)
</p>

`AS`로 정의된 칼럼 이름 조차 입력하기 귀찮은 사람들을 위해 다른 방법도 마련되어 있다. SELECT 문에 제시된 순서대로 칼럼을 지칭하는 방법이다. 굳이 `AS`로 따로 이름 붙이지 않아도 간단하고 직관적인 함수를 그대로 칼럼으로 이용하고자 할 때도 유용하다.
```sql
SELECT ROUND(imdb_rating),
   COUNT(name)
FROM movies
GROUP BY 1
ORDER BY 1;
```

> `WHERE`과 `HAVING`에서는 위 방법처럼 select 문 내의 순서로 칼럼을 지칭하는 방법이 통하지 않는다. 그 이유는 조건을 거는 where문과 having문의 특성 상 select문에 등장하지 않은 칼럼도 파라미터로 전달할 수 있기 때문이다. 아래 코드는 직원이 500보다 많은 스타트업들의 상호명과 위치를 조회한다.
```sql
SELECT name, location
FROM startups
WHERE employees > 500;
```

## 결론 
SQL의 기본 문법은 `SELECT ... FROM` 구문에서 시작하여 뻗어나간다. 즉, '어떤 테이블에서 어떤 칼럼을 선택하라'는 로직을 뼈대로 하여 여기에 원하는 여러 조건과 형태를 __새로운 칼럼을 정의하고 지칭하는 방식으로__ 붙여나간다. 여러 개의 테이블을 동시에 조작하고 subquery 등을 다루는 단계로 넘어가면 다른 화려한 트릭들이 눈에 들어오겠지만, 이 기본적인 틀을 유념하면 '아 이거 로직은 알겠는데 그냥 csv로 불러와서 파이썬에서 하면 안돼?'와 같은 (과거의 나 같은) 어리석음은 막을 수 있지 않을까 한다!