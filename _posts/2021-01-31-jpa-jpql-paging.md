---
title: JPQL) 페이징 API
author: cotchan 
date: 2021-01-31 03:09:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 페이징 API

+ JPA는 페이징을 다음 두 API로 추상화합니다.

+ **setFirstResult(int startPosition)**
  + 조회 시작 위치(0부터 시작)

+ **setMaxResults(int maxResult)**
  + 조회할 데이터 수

---

## 2. 페이징 API 예시

```java
List<Member> result = em.createQuery("select m from Member m order by m.age desc", Member.class)
    .setFirstResult(0)
    .setMaxResults(10)
    .getResultList();
```

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-paging-01.png)

---

+ MYSQL 방언
  + MYSQL은 아래 방언으로 쿼리가 나가게 됩니다.

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-paging-02.png)

---

+ Oracle 방언
  + Oracle은 아래 방언으로 쿼리가 나가게 됩니다.

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-paging-03.png)


---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
