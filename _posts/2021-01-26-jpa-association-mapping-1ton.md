---
title: JPA) 연관관계 매핑3. 일대다[1:N]
author: cotchan 
date: 2021-01-26 04:35:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 일대다[1:N] 매핑

![Desktop View](/assets/img/post/jpa/2021-01-26-jpa-association-mapping-1ton_03.png)

---

## 1. 일대다[1:N] 단방향

+ 연관관계의 주인이 `일`인 경우
  + 즉, `일`방향에서 외래키를 관리하겠다는 것을 의미합니다.

![Desktop View](/assets/img/post/jpa/2021-01-26-jpa-association-mapping-1ton_01.png)

---

+ **위 모델은 권장하지 않습니다.**
  + 다만 JPA 표준 스펙에서 지원합니다.
  + Team을 중심으로 외래키를 관리하겠다는 의미입니다.

+ **DB 입장에서는 무조건 `다`쪽에 외래키가 들어가야합니다.**
  + Team에는 외래키가 없고, Member에 외래키가 들어가야 합니다.
  + 그러므로 Team에 List members가 바뀌는 경우, MEMBER 테이블을 UPDATE해줘야 합니다.

---

+ Team.java

```java
@Entity
public class Team {

    @Id
    @GeneratedValue
    @Column(name = "TEAM_ID")
    private Long id;

    private String name;

    @OneToMany
    @JoinColumn(name = "TEAM_ID")
    private List<Member> members = new ArrayList<>();
}
```

---

+ Member.java

```java
@Entity
public class Member {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @Column(name = "USERNAME")
    private String username;
}
```

---

## 1-1. 일대다[1:N] 단방향 단점

+ **이 관계의 단점은 Team 엔티티에만 손을 댔는데, 쿼리를 추적해보면 Update MEMBER가 나갑니다.**
  + 테이블이 수십개 엮여서 돌아가는 환경에서 이런 점을 파악하는 것은 어렵습니다.
  + **그러므로 일대다 단방향은 `다대일 양방향 관계`로 만드는 게 낫습니다.** 

```java
//main

Member member = new Member();

member.setUsername("member1");
em.persist(member);

Team team = new Team();
team.setName("teamA");
team.getMembers().add(member);
em.persist(team);

tx.commit();
```

---

+ 위 코드를 실행했을 때 생성되는 JPA Query 

```java
Hibernate: 
    /* insert jpabook.jpashop.domain.Member
        */ insert 
        into
            Member
            (USERNAME, MEMBER_ID) 
        values
            (?, ?)
Hibernate: 
    /* insert jpabook.jpashop.domain.Team
        */ insert 
        into
            Team
            (name, TEAM_ID) 
        values
            (?, ?)
Hibernate: 
    /* create one-to-many row jpabook.jpashop.domain.Team.members */ update
        Member 
    set
        TEAM_ID=? 
    where
        MEMBER_ID=?
```

---

## 1-2. 일대다[1:N] 단방향 정리

+ 일대다 단방향은 일대다(1:N)에서 **일(1)이 연관관계의 주인입니다.**
+ 테이블에서의 일대다 관계는 항상 **다(N) 쪽에 외래 키가 있습니다.**
+ 일대다 단방향은 객체와 테이블의 차이 때문에 반대편 테이블의 외래 키를 관리하는 특이한 구조
+ `@JoinColumn`을 꼭 사용해야 합니다. 그렇지 않으면 조인 테이블 방식을 사용합니다.
  + 중간에 테이블을 하나 추가

+ **단점**
  + **엔티티가 관리하는 외래 키가 다른 테이블에 있습니다.**
  + 연관관계 관리를 위해 추가로 UPDATE_SQL을 실행합니다.
  
+ **그러므로 일대다 단방향 매핑보다는 `다대일 양방향 매핑을 사용`하는 게 낫습니다.**

---

## 2. 일대다[1:N] 양방향 관계

+ **이런 매핑은 공식적으로 존재 X**
  + 다대일 양방향을 사용하도록 합시다. :)

![Desktop View](/assets/img/post/jpa/2021-01-26-jpa-association-mapping-1ton_02.png)

---

+ **@JoinColumn(`insertable=false, updateable=false`)**을 사용합니다.
+ **읽기 전용 필드**를 사용해서 양방향처럼 사용하는 방식입니다.

+ Member.java 수정

```java
@Entity
public class Member {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @Column(name = "USERNAME")
    private String username;

    @ManyToOne
    @JoinColumn(name="TEAM_ID", insertable = false, updatable = false)
    private Team team;
}
```

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
