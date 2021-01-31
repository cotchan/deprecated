---
title: JPQL) 페치 조인1 - 기본
author: cotchan 
date: 2021-01-31 19:50:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 페치 조인

+ **실무에서 정말 정말 중요합니다.**

+ SQL 조인 종류 X
+ JPQL에서 **성능 최적화**를 위해 제공하는 기능입니다.
+ 연관된 엔티티나 컬렉션을 **SQL 한 번에 함께 조회**하는 기능
+ join fetch 명령어 사용

---

## 2. 엔티티 페치 조인

+ 회원을 조회하면서 연관된 팀도 함께 조회하고 싶을 때 (SQL 한 번에)

+ SQL을 보면 회원 뿐만 아니라 팀(T.*)도 함께 SELECT

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-fetchjoin-01.png)

+ JPQL에서 select projection에 m만 적었는데 실제 실행된 SQL은 M의 데이터와 T의 데이터를 전부 가져옵니다.
  
---

## 2-1. 예제 시나리오

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-fetchjoin-03.png)


---

## 2-2. 페치 조인 사용 코드

+ ManyToOne에 대한 페치 조인 예시
+ **참고로 연관관계로 지연 로딩을 셋팅해도 `페치 조인이 우선`합니다.**

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-fetchjoin-02.png)

---

## 2-3. 페치조인 사용 전

```java
String query = "select m From Member m";
List<Member> members = em.createQuery(query, Member.class)
    .getResultList();

for (Member member : members) {
    System.out.println("username = " + member.getUsername() + ", " + "teamName + " + member.getTeam().getName());
    //회원1, 팀A(SQL이 나가서 조회 => 팀A 정보 가져옴)
    //회원2, 팀A(팀A 정보를 영속성 컨텍스트 1차 캐시에서 가져옴, 그래서 그냥 출력된 것)
    //회원3, 팀B(팀B 정보는 1차 캐시에 없었으므로 SQL이 나가서 조회)

    //쿼리가 현재 총 3번 나감
    //회원 100명이면 => N+1 문제 발생
}
```

---

```java
//SQL
Hibernate: 
    /* select
        m 
    From
        Member m */ select
            member0_.MEMBER_ID as MEMBER_I1_0_,
            member0_.age as age2_0_,
            member0_.TEAM_ID as TEAM_ID4_0_,
            member0_.username as username3_0_ 
        from
            Member member0_
Hibernate: 
    select
        team0_.TEAM_ID as TEAM_ID1_3_0_,
        team0_.name as name2_3_0_ 
    from
        Team team0_ 
    where
        team0_.TEAM_ID=?
username = 회원1, teamName + 팀A
username = 회원2, teamName + 팀A
Hibernate: 
    select
        team0_.TEAM_ID as TEAM_ID1_3_0_,
        team0_.name as name2_3_0_ 
    from
        Team team0_ 
    where
        team0_.TEAM_ID=?
username = 회원3, teamName + 팀B
```

---

## 2-4. 페치조인 사용 후

```java
String query = "select m from Member m join fetch m.team";
//"멤버를 조회하긴 하는데, 한 번에 같이 fetch로 team을 가지고 와"라는 의미입니다.

//페치 조인을 한 시점에 이미 Member와 Team의 데이터를 다 들고 왔습니다.
//쿼리가 날라가서 Result가 담기는 시점에 프록시가 아니라 실제 엔티티가 담기게 됩니다. (영속성 컨텍스트에 다 올라가 있음)
List<Member> members = em.createQuery(query, Member.class)
    .getResultList();

for (Member member : members) {
    //그렇기 때문에 루프를 돌 때 별도의 쿼리가 나가지 않습니다.
    System.out.println("username = " + member.getUsername() + ", " + "teamName + " + member.getTeam().getName());
}
```

---

```java
//AFTER: 페치 조인 적용 후
Hibernate: 
    /* select
        m 
    from
        Member m 
    join
        fetch m.team */ select
            member0_.MEMBER_ID as MEMBER_I1_0_0_,
            team1_.TEAM_ID as TEAM_ID1_3_1_,
            member0_.age as age2_0_0_,
            member0_.TEAM_ID as TEAM_ID4_0_0_,
            member0_.username as username3_0_0_,
            team1_.name as name2_3_1_ 
        from
            Member member0_ 
        inner join
            Team team1_ 
                on member0_.TEAM_ID=team1_.TEAM_ID
username = 회원1, teamName + 팀A
username = 회원2, teamName + 팀A
username = 회원3, teamName + 팀B
```

---

## 3. 컬렉션 페치 조인

+ 일대다 관계나 컬렉션일 때 페치조인하는 방법
+ Team 입장에서 반대로 Member를 조인

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-fetchjoin-04.png)

---

## 3-1. 에제 시나리오

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-fetchjoin-05.png)

---

## 3-2. 컬렉션 페치 조인 사용 코드

+ **일대다 조인은 데이터가 뻥튀기 될 수 있습니다.**
  + `일의 입장`에서 fetch join으로 다 데이터를 끌고 오는 경우(Team => Member)
  + 반대로 다대일로의 조인은 데이터 뻥튀기가 안됩니다!

+ 위의 예제 시나리오 사진을 보면 Team 입장에서는 Member가 두 명 있습니다.
  + 조인을 하게 되면 [TEAM JOIN MEMBER] 처럼 두 개의 ROW가 생깁니다.
  + 즉, TEAM A 입장에서는 자기 1개인데, 멤버가 두 명이라 두 줄이 됩니다.
  + 또한 fetch join을 했기 때문에 Team A 입장에서는 두 개의 회원을 가지고 있습니다.

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-fetchjoin-06.png)

+ 팀은 두 개를 셋팅했는데 총 3개의 ROW가 나왔습니다. 

```java
Team teamA = new Team();
teamA.setName("팀A");
em.persist(teamA);

Team teamB = new Team();
teamB.setName("팀B");
em.persist(teamB);

Member member1 = new Member();
member1.setUsername("회원1");
member1.setTeam(teamA);
em.persist(member1);

Member member2 = new Member();
member2.setUsername("회원2");
member2.setTeam(teamA);
em.persist(member2);

Member member3 = new Member();
member3.setUsername("회원3");
member3.setTeam(teamB);
em.persist(member3);

em.flush();
em.clear();

String query = "select t from Team t join fetch t.members";

List<Team> result = em.createQuery(query, Team.class)
    .getResultList();

for (Team team : result) {
    System.out.println("team = " + team.getName() + "| members = "+ team.getMembers().size());
}
```

---

```java
//SQL
Hibernate: 
    /* select
        t 
    from
        Team t 
    join
        fetch t.members */ select
            team0_.TEAM_ID as TEAM_ID1_3_0_,
            members1_.MEMBER_ID as MEMBER_I1_0_1_,
            team0_.name as name2_3_0_,
            members1_.age as age2_0_1_,
            members1_.TEAM_ID as TEAM_ID4_0_1_,
            members1_.username as username3_0_1_,
            members1_.TEAM_ID as TEAM_ID4_0_0__,
            members1_.MEMBER_ID as MEMBER_I1_0_0__ 
        from
            Team team0_ 
        inner join
            Member members1_ 
                on team0_.TEAM_ID=members1_.TEAM_ID
team = 팀A| members = 2
team = 팀A| members = 2
team = 팀B| members = 1
```

---

## 4. 페치 조인과 DISTINCT

+ 중복이 싫은 경우, 중복은 DISTINCT로 제거할 수 있습니다.

+ JPQL의 DISTINCT는 두 가지 기능을 제공합니다.
  1. SQL에 DISTINCT를 추가
  2. 애플리케이션에서 엔티티 중복 제거

---

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-fetchjoin-07.png)

+ 애플리케이션에 결과가 올라올 때 이 컬렉션에 담겨있는 애가 중복이니까 JPA가 한 번 더 걸러준 꼴입니다.

```java
String query = "select distinct t from Team t join fetch t.members";
```

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-fetchjoin-08.png)

---

## 5. 페치 조인과 일반 조인의 차이

+ JPQL은 결과를 반환할 때 연관관계를 고려하지 않습니다.
+ **단지 SELECT 절에 지정한 엔티티만 조회**할 뿐입니다.
  + 일반 조인에서는 팀 엔티티만 조회하고, 회원 엔티티는 조회 X(지연 로딩)

+ 페치 조인을 사용할 때만 연관된 엔티티도 함께 조회합니다.(즉시 로딩)
  + **페치 조인을 사용한다는 건 즉시 로딩이 일어난다는 뜻입니다.**
+ **페치 조인은 객체 그래프를 SQL 한번에 조회하는 개념** 

---

## 5-1. 일반 조인의 경우

+ 실제 쿼리는 JOIN문이 나가지만 **실제 데이터를 퍼올리는 건 Team에 대한 데이터만 퍼올립니다.**

+ **즉, 연관 데이터가 로딩 시점에 로딩되어 있지 않습니다.**

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-fetchjoin-09.png)

---

```java
String query = "select t from Team t join t.members m";
```

```java
select
    team0_.TEAM_ID as TEAM_ID1_3_,
    team0_.name as name2_3_ 
from
    Team team0_ 
inner join
    Member members1_ 
        on team0_.TEAM_ID=members1_.TEAM_ID
```

---

+ 문제가 되는 건 실제 데이터를 사용하는 Loop를 돌릴 때 데이터가 아직 초기화가 되어있지 않습니다.
  + 데이터가 없기 때문에 값 조회시 추가적으로 쿼리가 나가는 것을 볼 수 있습니다.

```java
String query = "select t from Team t join t.members m";

List<Team> result = em.createQuery(query, Team.class)
    .getResultList();

for (Team team : result) {
    System.out.println("team = " + team.getName() + "| members = "+ team.getMembers().size());
}
```

---

```java
//SQL
Hibernate: 
    select
        members0_.TEAM_ID as TEAM_ID4_0_0_,
        members0_.MEMBER_ID as MEMBER_I1_0_0_,
        members0_.MEMBER_ID as MEMBER_I1_0_1_,
        members0_.age as age2_0_1_,
        members0_.TEAM_ID as TEAM_ID4_0_1_,
        members0_.username as username3_0_1_ 
    from
        Member members0_ 
    where
        members0_.TEAM_ID=?
team = 팀A| members = 2
team = 팀A| members = 2
Hibernate: 
    select
        members0_.TEAM_ID as TEAM_ID4_0_0_,
        members0_.MEMBER_ID as MEMBER_I1_0_0_,
        members0_.MEMBER_ID as MEMBER_I1_0_1_,
        members0_.age as age2_0_1_,
        members0_.TEAM_ID as TEAM_ID4_0_1_,
        members0_.username as username3_0_1_ 
    from
        Member members0_ 
    where
        members0_.TEAM_ID=?
team = 팀B| members = 1
```

---

## 5-2. 페치 조인의 경우

+ **쿼리가 한 번 나갈 때 연관된 데이터를 전부 퍼올립니다.**

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-fetchjoin-10.png)

---

```java
String query = "select t from Team t join fetch t.members m";
```

```java
Hibernate: 
    /* select
        t 
    from
        Team t 
    join
        fetch t.members m */ select
            team0_.TEAM_ID as TEAM_ID1_3_0_,
            members1_.MEMBER_ID as MEMBER_I1_0_1_,
            team0_.name as name2_3_0_,
            members1_.age as age2_0_1_,
            members1_.TEAM_ID as TEAM_ID4_0_1_,
            members1_.username as username3_0_1_,
            members1_.TEAM_ID as TEAM_ID4_0_0__,
            members1_.MEMBER_ID as MEMBER_I1_0_0__ 
        from
            Team team0_ 
        inner join
            Member members1_ 
                on team0_.TEAM_ID=members1_.TEAM_ID
```

---

```java
String query = "select t from Team t join fetch t.members m";

List<Team> result = em.createQuery(query, Team.class)
    .getResultList();

for (Team team : result) {
    System.out.println("team = " + team.getName() + "| members = "+ team.getMembers().size());
}
```

---

```java
//별도의 추가 쿼리가 나가지 않고 1차 캐쉬에서 값을 바로 가져옵니다.
team = 팀A| members = 2
team = 팀A| members = 2
team = 팀B| members = 1
```

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
