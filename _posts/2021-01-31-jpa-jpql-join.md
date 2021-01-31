---
title: JPQL) 조인 
author: cotchan 
date: 2021-01-31 03:47:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 조인 

+ 실행되는 건 동일하지만, 차이점이 있다면 얘는 Entity를 중심으로 동작합니다.
  + **회원(엔티티)과 연관있는 team을 .(점)으로 표시합니다. (`m.team t`)**

+ 세타 조인의 경우 **연관관계가 없는 두 엔티티를 조인**할 때 사용합니다. 

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-join-01.png)

---

## 2. 코드 예시

---

## 2-1. inner join 

+ inner 키워드 생략 가능

```java
Team team = new Team();
team.setName("teamA");
em.persist(team);

Member member = new Member();
member.setUsername("member1");
member.setAge(10);
member.setTeam(team);

em.persist(member);

em.flush();
em.clear();

String query = "select m from Member m inner join m.team t";
List<Member> result = em.createQuery(query, Member.class)
    .getResultList();
```

---

## 2-2. outer join

+ outer 키워드 생략 가능 

```java
Team team = new Team();
team.setName("teamA");
em.persist(team);

Member member = new Member();
member.setUsername("member1");
member.setAge(10);
member.setTeam(team);

em.persist(member);

em.flush();
em.clear();

String query = "select m from Member m left outer join m.team t";
List<Member> result = em.createQuery(query, Member.class)
    .getResultList();
```

---

## 2-3. 세타 조인 

```java
Team team = new Team();
team.setName("teamA");
em.persist(team);

Member member = new Member();
member.setUsername("member1");
member.setAge(10);
member.setTeam(team);

em.persist(member);

em.flush();
em.clear();

String query = "select m from Member m, Team t where m.username = t.name";
List<Member> result = em.createQuery(query, Member.class)
    .getResultList();
```

```java
//세타 조인 시 SQL
select
    member0_.MEMBER_ID as MEMBER_I1_0_,
    member0_.age as age2_0_,
    member0_.TEAM_ID as TEAM_ID4_0_,
    member0_.username as username3_0_ 
from
    Member member0_ cross 
join
    Team team1_ 
where
    member0_.username=team1_.name
```

---

## 3. 조인 - ON 절

+ ON절을 활용한 조인(JPA 2.1부터 지원합니다.)
  1. 조인 대상 필터링 가능
  2. **연관관계 없는 엔티티 외부 조인 가능(하이버네이트 5.1부터)**
    + 예전에는 내부 조인만 가능했습니다.
    + **연관관계가 없는 엔티티도 외부조인을 할 수 있도록 바뀌었습니다.**

+ **ON절은 `JOIN 할 때의 조건을 주는 것`입니다.**

---

## 3-1. 조인 대상 필터링

+ 회원과 팀을 조인하면서, 팀 이름이 A인 팀만 조인하고 싶은 경우
  + 팀의 데이터를 조금 줄인 후에 조인하고 싶다는 얘기

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-join-02.png)

---

## 3-2. 연관관계가 없는 엔티티 외부 조인

+ 회원의 이름과 팀의 이름이 같은 대상을 외부 조인할 수 있습니다.

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-join-03.png)




---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
