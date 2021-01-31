---
title: JPQL) Named 쿼리
author: cotchan 
date: 2021-01-31 21:15:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## Named 쿼리란

+ 미리 정의해서 이름을 부여해두고 사용하는 JPQL 입니다.

+ 정적 쿼리만 가능합니다.

+ 어노테이션이나 XML에 정의할 수 있습니다.

+ **애플리케이션 로딩 시점에 초기화 후 재사용합니다.**
  + JPA나 하이버네이트가 SQL로 파싱해서 캐싱하고 있습니다.

+ **애플리케이션 로딩 시점에 쿼리를 검증합니다.**
  + 컴파일 타임에 NamedQuery 검증을 위해 파싱을 하기 때문에 오류가 컴파일 시점에 발생합니다.


---

## 어노테이션 사용 시

```java
//1. NamedQuery 선언
@Entity
@NamedQuery(
        name = "Member.findByUsername",
        query = "select m from Member m where m.username = :username"
)
public class Member {

    /...
}
```

---

```java
//2. NamedQuery 사용 시
List<Member> resultList = em.createNamedQuery("Member.findByUsername", Member.class)
    .setParameter("username", "회원1")
    .getResultList();
```

---

## XML 사용 시

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-named-query-01.png)

---

## Named 쿼리 환경에 따른 설정

+ XML이 항상 우선권을 가집니다.
+ 애플리케이션 운영 환경에 따라 다른 XML을 배포할 수 있습니다.


---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
