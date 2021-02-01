---
title: JPA) JPQL이란(JPQL 소개)
author: cotchan 
date: 2021-01-30 22:45:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql]
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. JPA는 다양한 쿼리 방법 지원

+ JPQL: 표준 문법
+ JPA Criteria, QueryDSL
  + 이 두 개는 자바 코드로 짜서 JPQL을 빌드해주는 Generator
+ 네이티브 SQL
  + 데이터베이스 종속적인 쿼리가 나가야할 때(표준 SQL 문법을 벗어난 내용)


---

## 2. JPQL 소개

+ JPA를 사용하면 엔티티 객체를 중심으로 개발을 해야 합니다.

+ 문제는 검색 쿼리

+ **검색을 할 때도 테이블이 아닌 엔티티 객체를 대상으로 검색해야 합니다.**

+ 모든 DB 데이터를 객체로 변환해서 검색하는 것은 불가능합니다.

+ 결국 애플리케이션이 필요한 데이터만 DB에서 불러오려면 결국 검색 조건이 포함된 SQL이 필요합니다.

---

+ 그래서 JPA는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어를 제공합니다.

+ SQL과 문법이 유사합니다.
  + SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN 지원

+ **차이점**
  + JPQL은 엔티티 객체를 대상으로 쿼리
  + SQL은 데이터베이스 테이블을 대상으로 쿼리

---

## 3. 사용 예시

```java
//Member는 테이블이 아니라 엔티티를 가리키는 것입니다.
//select m은 Member 자체를 조회한다는 의미입니다.
List<Member> result = em.createQuery("select m from Member m where m.name like '%kim%'",
    Member.class
).getResultList();
```

---

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-jpql-basic-01.png)

---

## 4. QueryDSL 소개

+ 문자가 아닌 자바코드로 JPQL을 작성할 수 있습니다.
+ JPQL 빌더 역할
+ 컴파일 시점에 문법 오류를 찾을 수 있습니다.
+ 동적쿼리 작성이 편리합니다.
+ **단순하고 쉬움**
+ **실무 사용 권장**

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-jpql-basic-02.png)

---

## 5. JDBC 직접 사용, SpringJdbcTemplate 등 

+ JPA를 사용하면서 JDBC 커넥션을 직접 사용하거나, 스프링 JdbcTemplate, 마이바티스등을 함꼐 사용 가능

+ **단 영속성 컨텍스트를 적절한 시점에 강제로 플러시 해줘야 합니다.**
  + 위 기술들은 JPA와 무관한 기술이기 때문에 강제로 플러시를 해줘야 합니다.
  + **flush가 날아가는 시점은 commit 직전 시점이나 entityManager를 통해 query가 나가기 직전 입니다.**

+ 예) JPA를 우회해서 SQL을 실행하기 직전에 영속성 컨텍스트 수동 플러시



---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
