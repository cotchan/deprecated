---
title: JPQL) 엔티티를 직접 사용할 때 
author: cotchan 
date: 2021-01-31 20:56:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 엔티티 직접 사용 - 기본 키 값

+ JPQL에서 엔티티를 직접 사용하면 **SQL에서 해당 엔티티의 기본키 값을 사용합니다.**

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-entity-01.png)

---

+ 파라미터로 넘길 때도 마찬가지입니다.
  + 엔티티로 넘기든, 식별자를 직접 넘기든 SQL 결과는 동일합니다.

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-entity-02.png)

```java
String query = "select m from Member m where m = :member";

Member findMember = em.createQuery(query, Member.class)
    .setParameter("member", member1)
    .getSingleResult();
```

---

```java
//실제 SQL은 MEMBER_ID로 식별하게 됩니다.
select
    member0_.MEMBER_ID as MEMBER_I1_0_,
    member0_.age as age2_0_,
    member0_.TEAM_ID as TEAM_ID4_0_,
    member0_.username as username3_0_ 
from
    Member member0_ 
where
    member0_.MEMBER_ID=?
```

---

## 2. 엔티티 직접 사용 - 외래 키 값

+ 회원과 연관된 팀이니까 Member가 Team에 대한 외래키 값을 들고 있습니다. 

+ 아래에 적혀있는 team이라는 건 TEAM_ID 입니다.

```java
String query = "select m from Member m where m.team";

//위 쿼리에서 m.team이라는 건 아래의 TEAM_ID를 말하는 겁니다.
@Entity
public class Member {

    //...

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "TEAM_ID")
    private Team team;
}
```


![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-entity-03.png)


---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
