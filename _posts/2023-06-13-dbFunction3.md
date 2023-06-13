---
layout: post
title: DB 변환함수
date: 2023-06-13
category: DB
---


# 형 변환 함수

 **- 각 데이터에 지정된 자료형을 바꾸어 주는 함수를 형 변환 함수라고 한다.**
 
 ```sql
 select empno, ename, empno + '500' from emp where ename = 'SMITH';
 ```


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/2d351a9f-091e-4072-84fd-23ff7d6b4b8c)


위와 같이 empno (숫자) 와 '500'(문자열) 을 더한 값을 출력했는데 값이 7869로 정확하게 나왔다.
이는 자동 **형 변환**이라고 불리는 **암시적 형 변환**이 발생했기 때문이다.
숫자처럼 생긴 문자 데이터는 숫자 데이터로 **암시적 형 변환**이 일어나지만 그 외의 경우에는 잘 동작하지 않는다.

```sql
select 'ABCD' + empno, empno from emp where enmae = 'SMITH';
```

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/97747ced-aaf1-4f45-8493-0780045c9263)

 

이외에 사용자가 직접 자료형을 지정해 주는 **명시적 형 변환** 방법이 있다.

 

# 1. TO_CHAR 함수

 **-날짜나 숫자 데이터를 문자로 변환해주는 함수**
 **-TO_CHAR( [날짜 데이터] , [출력되길 원하는 문자 형태] )**
 
 
 ### 날짜 데이터 -> 문자
 
SYSDATE 함수를 이용하면 현재 날짜를 확인할 수 있다.

```sql 
select sysdate from dual;
```

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/a0e4f64a-5f19-4996-9e82-075e20e5cef0)


이 때 날짜 데이터는 RR/MM/DD 의 형식으로 저장된다.

여기서 년도를 입력하는 방식에 YY와 RR이 있다.

YY방식 : 입력 년도를 오라클 서버의 현재 날짜와 동시대로 계산 (ex 입력값이 19라면 2019, 55라면 2055년으로 인식)

RR방식 : RR방식은 현재 년도에 따라서 입력하는 년도의 결과값이 다르게 나온다. 아래 표를 참고하자.


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/bac13324-a815-4e66-b461-5fae6b39b59c)


이것을 참고해 SYSDATE를 RR/MM/DD형식이아닌 YYYY/MM/DD 방식으로 출력해보자.

```sql
select to_char(sysdate, 'YYYY/MM/DD') as today from dual;
```

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/48c480d6-d3d0-4064-b3c6-1678e21ed002)


날짜 표현 방식은 다음 표를 참고하자
|형식|설명|
|---|---|
|CC	세기
|YYYY, RRRR|	연 (4자리 숫자)|
|YY, RR|	연 (2자리 숫자)|
|MM|	월 (2자리 숫자)|
|MON|	월 (언어별 월 이름 약자)|
|MONTH|	월 (언어별 월 이름)|
|DD|	일 (2자리 숫자)|
|DDD| 	1년 중 몇일째인가 (1~366)|
|DY| 	요일 (언어별 요일 이름 약자)|
|DAY|	요일 (언어별 요일 이름)|
|W|	1년 중 몇번째 주 (1~53)|


```sql
select to_char(sysdate, 'CC') as 세기,
    to_char(sysdate, 'YYYY') as 년,
    to_char(sysdate, 'MM') as 월,
    to_char(sysdate, 'MON') as 월_약자,
    to_char(sysdate, 'MONTH') as 월_이름,
    to_char(sysdate, 'DD') as 일,
    to_char(sysdate, 'DDD') as 몇일째,
    to_char(sysdate, 'DY') as 요일,
    to_char(sysdate, 'DAY') as 요일_이름,
    to_char(sysdate, 'W') as 몇번째주
    from dual;
```

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/bca203a2-edc0-46f6-bc1c-9f1d3c24e779)


```sql
select sysdate, to_char(sysdate, 'DY', 'NLS_DATE_LANGUAGE=KOREAN') as KOR,
    to_char(sysdate, 'DY', 'NLS_DATE_LANGUAGE=JAPANESE') as JAP,
    to_char(sysdate, 'DY', 'NLS_DATE_LANGUAGE=ENGLISH') as ENG,
    to_char(sysdate, 'DAY', 'NLS_DATE_LANGUAGE=KOREAN') as KOR,
    to_char(sysdate, 'DAY', 'NLS_DATE_LANGUAGE=JAPANESE') as JAP,
    to_char(sysdate, 'DAY', 'NLS_DATE_LANGUAGE=ENGLISH') as ENG
    from dual;
```


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/f9573f23-5113-4a2a-9206-79e42b01d549)


시간 형식도 마찬가지로 직접 지정하여 출력하는것이 가능하다.

```sql
select to_char(sysdate, 'YYYY/MM/DD HH24:MI:SS AM') as today from dual;
```

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/bd43997f-d654-4dfd-9d0d-0450f0819f33)


시간 표현 방식은 다음 표를 참고하자.

|형식|설명|
|---|---|
|HH24|	|24시간으로 표현한 시간|
|HH, HH12|	12시간으로 표현한 시간|
|MI|	분|
|SS|	초|
|AM, PM, A.M, P.M|	오전, 오후 표시|



### 숫자 데이터 -> 문자

|형식|설명|
|---|---|
|9| 숫자의 한 자리를 의미 (빈자리를 채우지 않음)|
|0|	빈 자리를 0으로 채움|
|$|	달러 표시를 붙여서 출력|
|L|	L(LOCALE) 지역 화폐 단위 기호를 붙여서 출력|
|.|	소수점 표시|
|,|	천 단위의 구분기호 표시|

```sql
select sysdate, to_char(sal, '$999,999') as "$",
    to_char(sal, 'L999,999') as L,
    to_char(sal, '999,999.00') as 소수점,
    to_char(sal, '000,999,999.00') as 빈자리_천단위,
    to_char(sal, '000999999.99') as 빈자리,
    to_char(sal, '999,999,000') as 천단위
    from emp;
```

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/12e6cdb7-a596-4fca-89f1-e19bd4ab54ce)



# 2.TO_NUMBER 함수

**- 문자 데이터를 숫자 데이터로 변환하는 함수**

**- TO_NUMBER ( [문자열 데이터] , [인식될 숫자 형태] )**

문자 데이터를 숫자 데이터로 형 변환 하여 연산해보자
```sql
select to_number('21,000','999,999') - to_number('15,400','999,999') from dual;
```

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/6130045b-8abd-4dbf-9c66-0ebeef26ba1d)



# 3. TO_DATE 함수
 **- 문자 데이터를 날짜 데이터로 변환하는 함수**

 **- TO_DATE( [문자열데이터] , [인식될 날짜형태] )**

 ```sql
 select to_date('2018-07-14', 'YYYY-MM-DD') as date1,
    to_date('20180714','YYYY-MM-DD') as date2 from dual;
 ```
 
 ![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/29c58ffc-6a23-40e4-8051-3d1dcfbd451d)


이 함수를 이용하여 특정 날짜 이후에 입사한 사람의 정보를 확인해보자

```sql
select * from emp where hiredate > to_date('810601','RR/MM/DD');
select * from emp where hiredate > to_date('19810601','YY/MM/DD');
```
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/0d6ecd1f-b592-415b-b990-4a166d979b60)


두 SQL문 모두 같은 결과가 나온다
```sql
select * from emp where hiredate > '810601';
```
위처럼 입력해도 같은 결과를 얻을 수 있는데, 이를 통해 암시적 형변환이 일어났음을 알 수 있다.



# 4. NVL 함수
 **- NULL 여부를 검사하는 함수**

 **- NVL( [검사할 데이터 또는 열] , [앞의 데이터가 NULL일 경우 반환할 데이터] )**
 
 사원들의 총 임금을 구하는 SQL문을 작성해보자. ( 총 임금 = 급여 + 보너스 )

```sql
select empno, ename, sal, comm, sal + comm, nvl(comm, 0), sal + nvl(comm, 0) from emp;
```

null끼리의 연산은 결과가 null이 나오므로 위의 경우 보너스가 없는 사원의 경우 sal+comm (급여+보너스) 도 null이 되어 나온다. 이를 해결하기 위해 null인 경우 0으로 대체해주고 이것을 sal(급여)에 더해주면 정상적인 총 임금을 구할 수 있다,

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/2269d756-a4ae-48aa-b99c-c4e4128f7d6c)


 **- NVL2( [검사할 데이터 또는 열] , [앞의 데이터가 NULL이 아닐 경우 반환할 데이터] ,**

   **[앞의 데이터가 NULL일 경우 반환할 데이터] )**
   
   
   
NVL2 함수는 null인경우와 null이 아닌 경우를 모두 관리해준다.

이를 통해 보너스가 있으면 O 아니면 X를 출력해보자
 
 ```sql
 select ename, sal, comm, nvl2(comm, 'O', 'X') from emp;
```

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/50391315-f2eb-4cab-86d9-21fa6afac7e8)


-------------------------------------
출처 : <https://sehyeok.tistory.com/90>
