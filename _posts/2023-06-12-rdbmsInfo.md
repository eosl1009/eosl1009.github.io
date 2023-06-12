---
layout: post
title: RDBMS(Relational Database Management System)란?
date: 2023-06-12
category: DB
---


# 데이터베이스란?

* 체계화된 데이터의 모임
* 여러 응용 시스템들의 통합된 정보를 저장하여, 운영할 수 있는 공용 데이터의 묶음
* 논리적으로 연관된 하나 이상의 자료 모음으로, 데이터를 고도로 구조화함으로써 검색/갱신등의 데이터 관리를 효율화함
*DBMS: 데이터베이스를 관리하는 시스템


* 데이터베이스 장점

  * 데이터 중복 최소화
  * 데이터 공유
  * 일관성, 무결성, 보안성 유지
  * 최신의 데이터 유지
  * 데이터의 표준화 가능
  * 데이터의 논리적, 물리적 독립성
  * 용이한 데이터 접근
  * 데이터 저장 공간 절약

* 데이터베이스 단점

  * 데이터베이스 전문가 필요
  * 많은 비용 부담
  * 시스템의 복잡함
  * 데이터베이스 랭킹 (Oct. 2017)



# RDBMS(Relational Database Management System, 관계형 데이터베이스 관리 시스템)

* 데이터베이스의 한 종류로, 가장 많이 사용됨
* 역사가 오래되어, 가장 신뢰성이 높고, 데이터 분류, 정렬, 탐색 속도가 빠름

* 관계형 데이터베이스 = 테이블!
* 2차원 테이블(Table) 형식을 이용하여 데이터를 정의하고 설명하는 데이터 모델
* 관계형 데이터베이스에서는 데이터를 속성(Attribute)과 데이터 값(Attribute Value)으로 구조화(2차원 Table 형태로 만들어짐)
* 데이터를 구조화한다는 것은 속성(Attribute)과 데이터 값(Attribute Value) 사이에서 관계(Relation)을 찾아내고 이를 테이블 모양의 구조로 도식화함의 의미함
* 주요 용어

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/2006db04-03c4-4718-8228-0474eb34616b)


  * Primary Key and Foreign Key
    * Primary Key(기본키): Primary Key는 한 테이블(Table)의 각 로우(Row)를 유일하게 식별해주는 컬럼(Column)으로,
    * 각 테이블마다 Primary Key가 존재해야 하며, NULL 값을 허용하지 않고, 각 로우(Row)마다 유일한 값이어야 한다.
  * Foreign Key(외래키 또는 외부키): Foreign Key는 한 테이블의 필드(Attribute) 중 다른 테이블의 행(Row)을 식별할 수 있는 키

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/8c877866-8903-47f8-a10c-e5a8bed615df)


# 데이터베이스 스키마(Schema)

* 데이터베이스의 테이블 구조 및 형식, 관계 등의 정보를 형식 언어(formal language)로 기술한 것
  * 관계형 데이터베이스를 사용하여 데이터를 저장할 때 가장 먼저 할 일은 데이터의 공통 속성을 식별하여 컬럼(Column)으로 정의하고, 테이블(Table)을 만드는 것
  * 통상적으로 하나의 테이블이 아닌 여러 개의 테이블로 만들고, 각 테이블 구조, 형식, 관계를 정의함
  * 이를 스키마라고 하며, 일종의 데이터베이스 설계도로 이해하면 됨
  * 데이터베이스마다 스키마를 만드는 언어가 존재하며, 해당 스키마만 있으면 동일한 구조의 데이터베이스를 만들 수 있음
(데이터베이스 백업과는 달리 데이터 구조만 동일하게 만들 수 있음)

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/26006c49-6910-47a5-9fc6-da2d14b034a8)


# SQL(Structured Query Language)

* 관계형 데이터베이스 관리 시스템에서 데이터를 관리하기 위해 사용되는 표준 프로그래밍 언어(Language)
* 데이터베이스 스키마 생성 및 수정, 테이블 관리, 데이터 추가, 수정, 삭제, 조회 등, 데이터베이스와 관련된 거의 모든 작업을 위해 사용되는 언어
* 데이터베이스마다 문법에 약간의 차이가 있지만, 표준 SQL을 기본으로 하므로, 관계형 데이터베이스를 다루기 위해서는 필수적으로 알아야 함
* SQL은 크게 세 가지 종류로 나뉨
    * 데이터 정의 언어(DDL, Data Definition Language)
    * 데이터 처리 언어(DML, Data Manipulation Language)
    * 데이터 제어 언어(DCL, Data Control Language)

## 데이터 정의 언어(DDL, Data Definition Language): 데이터 구조 정의

* 테이블(TABLE), 인덱스(INDEX) 등의 개체를 만들고 관리하는데 사용되는 명령
* CREATE, ALTER, DROP 등이 있음

## 데이터 조작 언어(DML, Data Manipulation Language): 데이터 CRUD [Create(생성), Read(읽기), Update(갱신), Delete(삭제)

* INSERT 테이블(Table)에 하나 이상의 데이터 추가.
* UPDATE 테이블(Table)에 저장된 하나 이상의 데이터 수정.
* DELETE 테이블(Table)의 데이터 삭제.
* SELECT 테이블(Table)에 저장된 데이터 조회.

##  데이터 제어 언어(DCL, Data Control Language): 데이터 핸들링 권한 설정, 데이터 무결성 처리 등 수행

* GRANT 데이터베이스 개체(테이블, 인덱스 등)에 대한 사용 권한 설정.
* BEGIN 트랜잭션(Transaction) 시작.
* COMMIT 트랜잭션(Transaction) 내의 실행 결과 적용.
* ROLLBACK 트랜잭션(Transaction)의 실행 취소.




---------------------------------------

춣처 : <https://www.fun-coding.org/mysql_basic1.html#gsc.tab=0>
