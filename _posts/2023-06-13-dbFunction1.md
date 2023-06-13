---
layout: post
title: DB 집계함수
date: 2023-06-13
category: DB
---


# 집계함수란?

> 집계 함수란 여러행 또는 테이블 전체 행으로부터 하나의 결과값을 반환하는 함수이다.


## 집계함수(Aggregate function)의 이해

* GROUP BY절을 이용하여 그룹 당 하나의 결과로 그룹화 할 수 있다.
* HAVING절을 사용하여 집계함수를 이용한 조건 비교를 할 수 있다.
* MIN, MAX 함수는 모든 자료형에 사용 할 수 있다.
* 일반적으로 가장 많이 사용하는 집계함수에는AVG(평균), COUNT(개수), MAX(최대값), MIN(최소값), SUM(합계) 등이 있다.

### COUNT

COUNT 함수는 검색된 행의 수를 반환 한다.

```sql
-- 검색된 행의 총 수 4개를 반환. 즉 4개의 부서가 존재한다.
SELECT COUNT(deptno) FROM dept;
 
COUNT(DEPTNO)
-------------
            4
```


### MAX

MAX 함수는 컬럼값 중에서 최대값을 반환 한다.

```sql
-- sal 컬럼값 중에서 제일 큰값을 반환. 즉 가장 큰 급여를 반환.
SELECT MAX(sal) salary FROM emp;
 
SALARY
-------
  5000 
```


### MIN

MIN 함수는 컬럼값 중에서 최소값을 반환 한다.

```sql
-- sal 컬럼값 중에서 가장 작은 값 반환. 즉 가장 적은 급여를 반환
SELECT MIN(sal) salary FROM emp;
 
 SALARY
-------
    800    
```

### AVG

AVG 함수는 평균 값을 반환 한다.

```sql
부서번호 30의 사원 평균 급여를 소수점 1자리 이하에서 반올림
SELECT ROUND(AVG(sal),1) salary 
  FROM emp 
 WHERE deptno = 30;
 
 SALARY
 ------
 1566.7
```

### SUM

SUM 함수는 검색된 컬럼의 합을 반환 한다.

```sql
- 부서번호 30의 사원 급여 합계를 조회.
SELECT SUM(sal) salary 
  FROM emp 
 WHERE deptno = 30;
 
 SALARY
-------
   9400    
```

### STDDEV

STDDEV 함수는 표준편차를 반환 한다.

```sql
-- 부서번호 30의 사원 급여 표준편차를 반환.    
SELECT ROUND(STDDEV(sal),3) salary 
  FROM  emp 
 WHERE deptno = 30;
 
  SALARY
--------
 668.331  
```

### 집계함수 예

아래는 부서별 사원수, 최대급여, 최소급여, 급여합계, 평균급여를 급여합게 순으로 조회하는 예이다.

```sql
SELECT deptno 부서번호, COUNT(*) 사원수, 
       MAX(sal) 최대급여, MIN(sal) 최소급여, 
       SUM(sal) 급여합계, ROUND(AVG(sal)) 평균급여
  FROM emp
 GROUP BY deptno
 ORDER BY SUM(sal) DESC;
 
 
부서번호   사원수  최대급여  최소급여   급여합계   평균급여
------- -------- --------- -------- --------- ---------
     20       5      3000       800    10875     2175
     30       6      2850       950     9400     1567
     10       3      5000      1300     8750     2917

```

--------------------------------------------

출처 <http://www.gurubee.net/lecture/1031>
