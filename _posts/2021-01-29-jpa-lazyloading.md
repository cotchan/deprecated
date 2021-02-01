---
title: JPA) 즉시 로딩과 지연 로딩
author: cotchan 
date: 2021-01-29 23:30:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. Intro

![Desktop View](/assets/img/post/jpa/2021-01-29-jpa-lazyloading-01.png)

---

## 2. Lazy Loading

+ **결론**
  + 지연로딩으로 셋팅하면 **연관된 엔티티를 `프록시로 가져옵니다.`**


+ **지연 로딩 LAZY를 사용해서 프록시로 조회**

![Desktop View](/assets/img/post/jpa/2021-01-29-jpa-lazyloading-02.png)

---

+ **이렇게 fetch 옵션을 달아주면 프록시 객체로 조회하게 됩니다.**
  + **즉, `멤버클래스만 조회`를 하게 됩니다.**

```java
@Entity
public class Member extends BaseEntity {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name= "TEAM_ID", insertable = false, updatable = false)
    private Team team;
}
```

---

## 2-1. 지연 로딩 동작 매커니즘

![Desktop View](/assets/img/post/jpa/2021-01-29-jpa-lazyloading-03.png)

---

+ find하는 시점에는 가짜 프록시 객체를 연결해놓습니다.
+ 그다음에 Team의 값을 실제로 사용하는 시점에 초기화가 되면서 DB에 쿼리가 나가게 됩니다.
  + **중요한 건, Team을 가져올 때가 아니라 Team에 있는 무언가를 실제 사용하는 시점에 초기화됩니다.**
  + `getTeam()`은 그냥 프록시 객체를 가져오는 것입니다.

![Desktop View](/assets/img/post/jpa/2021-01-29-jpa-lazyloading-04.png)

---

## 2-2. 예시 코드

```java
Team team = new Team();
team.setName("teamA");
em.persist(team);

Member member1 = new Member();
member1.setUsername("member1");
member1.setTeam(team);

em.persist(member1);

em.flush();
em.clear();

Member findMember = em.find(Member.class, member1.getId());
System.out.println("findMember = " + findMember.getTeam().getClass());

System.out.println("=================");
findMember.getTeam().getName();
System.out.println("=================");
```

---

+ **멤버 따로, 팀 따로 쿼리가 나가는 것을 볼 수 있습니다.**

```java
//find를 통해 Member 조회
Hibernate: 
    select
        member0_.MEMBER_ID as MEMBER_I1_1_0_,
        member0_.createdBy as createdB2_1_0_,
        member0_.createdDate as createdD3_1_0_,
        member0_.lastModifiedBy as lastModi4_1_0_,
        member0_.lastModifiedDate as lastModi5_1_0_,
        member0_.TEAM_ID as TEAM_ID7_1_0_,
        member0_.USERNAME as USERNAME6_1_0_ 
    from
        Member member0_ 
    where
        member0_.MEMBER_ID=?

//아직 Team Entity의 메소드를 사용하지 않았으므로 Proxy 리턴
findMember = class jpabook.jpashop.domain.Team$HibernateProxy$2NVP6gDw
=================
//Team에서 getName() 호출하는 시점에, 프록시 초기화 후 Team에 대한 쿼리가 나갑니다.
Hibernate: 
    select
        team0_.TEAM_ID as TEAM_ID1_4_0_,
        team0_.createdBy as createdB2_4_0_,
        team0_.createdDate as createdD3_4_0_,
        team0_.lastModifiedBy as lastModi4_4_0_,
        team0_.lastModifiedDate as lastModi5_4_0_,
        team0_.name as name6_4_0_ 
    from
        Team team0_ 
    where
        team0_.TEAM_ID=?
=================
```

---

## 2-3. 지연 로딩은 언제 사용?

+ Member를 조회했을 때 대부분 Member만 사용하는 경우에 바람직합니다.
  + Member에 대해 Team을 자주 조회하지는 않을 때

+ 허나 비즈니스 로직상 Member와 Team을 자주 같이 사용한다면 Lazy Loading은 바람직하지 않습니다.
  + 그 때 마다 쿼리가 두 번씩 나가기 때문!

---

## 3. 즉시 로딩(EAGER)

+ 비즈니스 로직상 Member와 Team을 자주 같이 사용한다면 즉시로딩이 적합합니다.
  + **즉시 로딩 EAGER를 사용해서 함께 조회합니다.**

![Desktop View](/assets/img/post/jpa/2021-01-29-jpa-lazyloading-05.png)

---

## 3-1. 사용 예시 코드

+ 지연 로딩 => 즉시 로딩으로 바꿔준 경우

```java
@Entity
public class Member extends BaseEntity {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name= "TEAM_ID", insertable = false, updatable = false)
    private Team team;
}
```

---

+ **`em.find`로 가져올 때 연관된 엔티티 값을 다 가져와서 셋팅하는 것을 즉시 로딩이라고 합니다.**

```java
Team team = new Team();
team.setName("teamA");
em.persist(team);

Member member1 = new Member();
member1.setUsername("member1");
member1.setTeam(team);

em.persist(member1);

em.flush();
em.clear();

Member findMember = em.find(Member.class, member1.getId());
System.out.println("findMember = " + findMember.getTeam().getClass());

System.out.println("=================");
findMember.getTeam().getName();
System.out.println("=================");
```


---

```java
//find를 통해 Member를 조회하는 시점에 team도 다 가져오는 것을 볼 수 있습니다.

Hibernate: 
    select
        member0_.MEMBER_ID as MEMBER_I1_1_0_,
        member0_.createdBy as createdB2_1_0_,
        member0_.createdDate as createdD3_1_0_,
        member0_.lastModifiedBy as lastModi4_1_0_,
        member0_.lastModifiedDate as lastModi5_1_0_,
        member0_.TEAM_ID as TEAM_ID7_1_0_,
        member0_.USERNAME as USERNAME6_1_0_,
        team1_.TEAM_ID as TEAM_ID1_4_1_,
        team1_.createdBy as createdB2_4_1_,
        team1_.createdDate as createdD3_4_1_,
        team1_.lastModifiedBy as lastModi4_4_1_,
        team1_.lastModifiedDate as lastModi5_4_1_,
        team1_.name as name6_4_1_ 
    from
        Member member0_ 
    //애초에 가져올 때 Member와 Team을 join합니다.
    left outer join
        Team team1_ 
            on member0_.TEAM_ID=team1_.TEAM_ID 
    where
        member0_.MEMBER_ID=?

//그래서 getClass를 하면 진짜 Team 객체가 나옵니다.
findMember = class jpabook.jpashop.domain.Team
=================
//그래서 getName을 해도 별도의 쿼리 호출이 없는 것을 알 수 있습니다.
=================
```

---

## 3-2. 즉시 로딩 동작 매커니즘

![Desktop View](/assets/img/post/jpa/2021-01-29-jpa-lazyloading-06.png)

---

![Desktop View](/assets/img/post/jpa/2021-01-29-jpa-lazyloading-07.png)

---

## 4. 프록시와 즉시로딩 주의점

+ **실무에서는 즉시 로딩을 사용하면 안됩니다.**

+ 즉시 로딩을 적용하면 예상하지 못한 SQL이 발생합니다.

+ **즉시 로딩은 JPQL에서 `N+1` 문제를 일으킵니다.**

+ **@ManyToOne, @OneToOne은 `기본이 즉시 로딩`입니다.**
  + 그러므로 LAZY로 설정해줘야 합니다.
  + **`'X'toOne`이 이렇게 default로 셋팅되어 있습니다.**

+ @OneToMany, @ManyToMany는 기본이 지연 로딩입니다.

---

## 4-1. N+1 문제

+ 즉시 로딩이라는 말은 가져올 때 일단 무조건 값이 다 들어가 있어야 합니다.

+ 그러면 JPQL은 그러면 Member 쿼리가 나가고, Member를 가져온 후 나머지 값을 셋팅하기 위해 별도의 쿼리가 나가게 됩니다.

+ **N+1이란
  + N이 결과를 의미합니다.
  + 최초로 날리는 쿼리가 1을 의미합니다.
  + **최초 쿼리를 날렸는데 그것 때문에 추가 쿼리가 N개가 나간다고 해서 N+1이라고 합니다.**

---

## 5. 지연 로딩 활용

+ **실무에서는 `전부 지연 로딩`으로 셋팅해야 합니다.**
+ 아래 내용은 이론 적인 것입니다.

![Desktop View](/assets/img/post/jpa/2021-01-29-jpa-lazyloading-08.png)

---

![Desktop View](/assets/img/post/jpa/2021-01-29-jpa-lazyloading-09.png)

---

![Desktop View](/assets/img/post/jpa/2021-01-29-jpa-lazyloading-10.png)

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
