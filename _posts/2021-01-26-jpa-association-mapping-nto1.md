---
title: JPA) 연관관계 매핑3. 다대일[N:1]
author: cotchan 
date: 2021-01-26 04:30:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 연관관계 매핑시 고려사항 3 가지

+ 다중성
+ 단방향, 양방향
+ 연관관계의 주인

---

## 1-1. 다중성

+ **다대일**: `@ManyToOne`
+ **일대다**: `@OneToMany`
+ **일대일**: `@OneToOne`
+ **다대다**: `@ManyToMany`


+ 다중성은 DB랑 매핑하기 위해 존재합니다. 그러므로 DB 관점에서 다중성을 기준으로 고민하면 됩니다. 
+ **매핑관계 판단이 애매하다면 반대 측면을 생각해보면 됩니다.**
  + 일대일의 반대는 일대일
  + 다대다의 반대는 다대다
  + 일대다의 반대는 다대일

+ **다대다는 실무에서 사용하면 안됩니다.**

---

## 1-2. 단방향, 양방향

+ **테이블**
  + 외래 키 하나로 양쪽 조인 가능
  + 사실 방향이라는 개념이 없습니다.

+ **객체**
  + **객체는 `참조용 필드`가 있는 쪽으로만 참조가 가능합니다.**
  + 한쪽만 참조하면 단방향
  + 양쪽이 서로 참조하면 양방향
    + 좀 더 엄밀히 말하면 `양방향`이라는 개념보다는 `단방향이 2개`입니다.

---

## 1-3. 연관관계의 주인

+ 테이블은 **외래 키 하나**로 두 테이블이 연관관계를 맺습니다.
+ 객체 양방향 관게는 A->B, B-A처럼 **참조가 2군데입니다.**
+ 객체 양방향 관계는 참조가 2군데 있습니다. 그러므로 둘 중 테이블의 외래 키를 관리할 곳을 지정해줘야합니다.
+ **`연관관계의 주인`: 외래 키를 관리하는 참조를 의미합니다.**
  + 주인의 반대편: 외래 키에 영향을 주지 않습니다. 단순 조회만 가능합니다.

---

## 2. 다대일[N:1] 매핑

![Desktop View](/assets/img/post/jpa/2021-01-25-jpa-association-mapping-nto1_03.png)

---

## 2-1. 다대일[N:1] - 단방향

+ **JPA에서 가장 많이 사용하는 연관관계입니다.**

+ **일대다 관계에서 항상 `다쪽에 연관관계의 주인`이 있어야합니다.**
  + ex. Member와 Team의 관계라면, Member에 외래키가 들어가줘야 합니다.
  + `외래키가 있는 곳`에 `참조를 매핑`하고 연관관계를 매핑하면 됩니다.

![Desktop View](/assets/img/post/jpa/2021-01-25-jpa-association-mapping-nto1_01.png)

---

## 2-1. 다대일 단방향 코드 예시

+ `Member.java`

```java
@Entity
public class Member {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @Column(name = "USERNAME")
    private String username;

    @ManyToOne  //관계가 무엇인지?
    @JoinColumn(name = "TEAM_ID") //이 관계에서 조인하는 컬럼은 무엇인지?
    private Team team;
}
```

---

+ `Team.java`
  + Team에는 Member로 가는 게 없습니다.

```java
@Entity
public class Team {

    @Id
    @GeneratedValue
    @Column(name = "TEAM_ID")
    private Long id;

    private String name;
}
```

---

## 2-2. 다대일[N:1] - 양방향

+ 반대쪽 사이드에 참조를 추가한다고 해도 **테이블에 전혀 영향을 주지 않습니다.**
  + 어차피 연관관계의 주인으로 외래키를 관리하고 있습니다.
+ 외래 키가 있는 쪽이 연관관계의 주인입니다.
+ 양쪽을 서로 참조하도록 개발합니다.

![Desktop View](/assets/img/post/jpa/2021-01-25-jpa-association-mapping-nto1_02.png)

---

## 2-2. 다대일 양방향 코드 예시

+ `Team.java`
  + Team 엔티티에 Member를 참조할 수 있도록 멤버 필드를 추가해줍니다.

```java
@Entity
public class Team {

    @Id
    @GeneratedValue
    @Column(name = "TEAM_ID")
    private Long id;

    private String name;

    //나는 'team'에 의해서 관리가 되어지는 필드입니다.
    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<>();
}
```


---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
