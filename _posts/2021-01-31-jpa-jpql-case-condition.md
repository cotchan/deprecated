---
title: JPQL) 조건식(CASE 등) 
author: cotchan 
date: 2021-01-31 04:24:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 조건식 - CASE 식 

+ 기본 CASE식 - 범위 조회
+ 단순 CASE식 - 특정 값 조회

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-case-condition-01.png)

---

+ 에시

```java
String query =
    "select" +
        "case when m.age <= 10 then '학생요금' " +
        "     when m.age >= 60 then '경로요금' " +
        "     else '일반요금' " +
        "end " +
    "from Member m";

List<String> result = em.createQuery(query, String.class)
    .getResultList();
```

---

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-case-condition-02.png)


---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
