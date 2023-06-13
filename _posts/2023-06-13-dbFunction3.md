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
