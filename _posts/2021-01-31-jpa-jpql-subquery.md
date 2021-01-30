---
title: JPQL) 서브쿼리
author: cotchan 
date: 2021-01-31 03:50:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 서브 쿼리

+ 서브 쿼리 예시

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-subquery-01.png)

---

## 2. 서브 쿼리 지원 함수

+ **[NOT] `EXISTS`**: 서브쿼리에 결과가 존재하면 참
  + ALL, ANY, SOME을 섞어서 사용할 수 있습니다.
    + ALL: 모두 만족하면 참
    + ANY, SOME: 같은 의미입니다. 조건을 하나라도 만족하면 참

+ **[NOT] `IN`**: 서브쿼리의 결과 중 하나라도 같은 것이 있으면 참

---

## 3. 서브 쿼리 - 예제

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-subquery-02.png)

---

## 4. JPA 서브 쿼리 한계

+ JPA는 WHERE, HAVING 절에서만 서브 쿼리를 사용할 수 있습니다.
+ SELECT절도 가능합니다. (하이버네이트에서 지원)
+ **단, FROM 절의 서브 쿼리는 현재 JPQL에서는 불가능합니다.**
  + 조인으로 풀 수 있으면 풀어서 해결하도록 합니다.

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
