---
layout: post
title: DB 날짜함수
date: 2023-06-13
category: DB
---


# 날짜 함수

날짜 함수는 DATE 함수나 TIMESTAMP 함수와 같은 날짜형을 대상으로 연산을 수행해 결과를 반환하는 함수다. 날짜 함수 역시 대부분 반환 결과는 날짜형이나 함수에 따라 숫자를 반환할 때도 있다.


## 1. SYSDATE, SYSTIMESTAMP

SYSDATE와 SYSTIMESTAMP는 현재일자와 시간을 각각 DATE, TIMESTAMP 형으로 반환한다.


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/704c8a37-2f31-4a11-97ff-c438d611df95)


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/d7648b5a-bb3f-48af-8f91-c261f2198dde)



### 2. ADD_MONTHS (date, integer)

ADD_MONTHS 함수는 매개변수로 들어온 날짜에 interger 만큼의 월을 더한 날짜를 반환한다.


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/5622c379-f303-49ba-9419-ffb25773c126)


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/7a1421f1-fb83-4598-aace-747b5457e005)


### 3. MONTHS_BETWEEN(date1, date2)

MONTHS_BETWEEN 함수는 두 날짜 사이의 개월 수를 반환하는데, date2가 date1보다 빠른 날짜가 온다.


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/b2066872-ccb3-4a66-a1d9-3e8f9e2dc719)


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/fc82996e-a539-48d7-8508-d4ec7a054f5e)


### 4. LAST_DAY(date)

LAST_DAY는 date 날짜를 기준으로 해당 월의 마지막 일자를 반환한다.


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/5d3f7385-f8de-47a3-b109-c71dcb69477b)


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/011949be-0e7f-4012-a211-16ee6f6a854f)


### 5. ROUND(date, format), TRUNC(date, format)

ROUND와 TRUNC는 숫자 함수이면서 날짜 함수로도 쓰이는데, ROUND는 format에 따라 반올림한 날짜를, TRUNC는 잘라낸 날짜를 반환한다.


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/a1edce64-8757-47f9-bb1a-122e072285b4)


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/0962639e-a303-4286-a931-2b477b201b8e)


### 6. NEXT_DAY (date, char)

NEXT_DAY는 date를 char에 명시한 날짜로 다음 주 주중 일자를 반환한다.


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/5f49aac9-af10-4d88-8a86-632be3b52b46)


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/28521a05-5c44-4896-8a14-0931f8c75d93)


------------------------

출처 : <https://sumingkk.tistory.com/41>
