---
title: JPQL) 다형성 쿼리
author: cotchan 
date: 2021-01-31 20:44:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. INTRO

+ 아래와 객체 관계를 가정합니다.

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-poly-01.png)

---

## 2. TYPE

+ SQL로 바뀔 때 DTYPE(구분 컬럼)으로 바뀝니다.

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-poly-02.png)

---

## 3. TREAT(JPA 2.1)

+ 자바의 타입 캐스팅과 유사합니다.
+ 상속 구조에서 부모 타입을 특정 자식 타입으로 다룰 때 사용합니다.
+ FROM, WHERE, SELECT(하이버네이트 지원) 사용

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-poly-03.png)


---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
