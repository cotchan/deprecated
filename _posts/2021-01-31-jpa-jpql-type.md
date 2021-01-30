---
title: JPQL) 타입 표현과 기타식
author: cotchan 
date: 2021-01-31 04:05:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. JPQL 타입 표현

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-type-01.png)

---

+ **ENUM**
  + ENUM같은 경우는 하드 코딩을 하는 경우 꼭 패키지명을 다 포함해서 넣어줘야 합니다.

```java
//예시
String query = "select m.username From Member m where m.type = jpql.MemberType.ADMIN";
String query = "select m.username From Member m where m.type = jpql.MemberType.USER";
```

+ 하드 코딩을 해야 하는 경우에 패키지명을 다 적어줘야 하는 것입니다.
+ 보통은 다음과 같이 파라미터 바인딩으로 사용합니다.

```java
String query = "select m.username From Member m where m.type = :userType";

em.createQuery(query)
.setParameter("userType", MemberType.ADMIN)
.getResultList();
```

---

+ **엔티티 타입**

```java
//쿼리 사용 예시
em.createQuery("select i from Item i where type(i) == Book", Item.class)
.getResultList();
```

---

## 2. JPQL 기타식

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-type-02.png)


---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
