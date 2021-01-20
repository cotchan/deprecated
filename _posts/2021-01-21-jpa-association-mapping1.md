---
title: JPA) 연관관계 매핑1. 단방향 연관 관계
author: cotchan 
date: 2021-01-21 04:30:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 주요 용어 설명

+ **방향**
  + 단방향, 양방향

+ **다중성**
  + 다대일(N:1)
  + 일대다(1:N)
  + 일대일(1:1)
  + 다대다(N:N)

+ **연관관계의 주인**
  + 객체의 `양방향 연관관계`는 관리 주인이 필요합니다.

---

## 2. 연관관계가 필요한 이유

+ 객체지향 설계의 목표는 자율적인 객체들의 `협력 공동체`를 만드는 것입니다.

---

+ 예제 시나리오
  1. 회원과 팀이 있습니다.
  2. 회원은 하나의 팀에만 소속될 수 있습니다.
  3. 회원과 팀은 다대일 관계입니다.

---

## 3. 객체를 테이블에 맞추어 모델링하는 경우

---

## 3-1. 도메인 설계

+ MEMBER와 TEAM의 관계는 `N:1` 입니다.
  + **`외래키를 필드`로 가지고 있다는 것은 연관관계에서 `N`이라는 것입니다.**

+ **테이블에 맞춰 외래키 값을 그대로 가지고 온 경우**

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-01.png)

---

## 3-2. 엔티티 코드

+ **참조 대신에 외래 키를 그대로 사용**

```java
@Entity
public class Member {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @Column(name = "USERNAME")
    private String username;

    @Column(name = "TEAM_ID")
    private Long teamId;
    //...
}

@Entity
public class Team {

    @Id
    @GeneratedValue
    @Column(name = "TEAM_ID")
    private Long id;

    private String name;
    //...
}
```

---

## 3-3. USECASE

+ 이렇게 모델링을 하면 **외래 키 식별자를 직접 다뤄줘야 합니다.**

```java
//팀 저장 
Team team = new Team(); 
team.setName("TeamA");  
em.persist(team); 

//회원 저장 
Member member = new Member(); 
member.setName("member1");  
member.setTeamId(team.getId());  
em.persist(member); 

//조회 
Member findMember = em.find(Member.class, member.getId());

//연관관계가 없음 
Team findTeam = em.find(Team.class, team.getId());
```

---

## 3-4. 문제점

+ 객체를 테이블에 맞추어 데이터 중심으로 모델링을 하면, **`협력관계`를 만들 수 없습니다.**

---

## 4. 테이블과 객체의 큰 차이점

+ 객체를 테이블에 맞추어 데이터 중심으로 모델링하면, 협력관계를 만들 수 없습니다.
+ **테이블은 `외래 키로 조인`을 사용해서 연관된 테이블을 찾습니다.**
+ **객체는 `참조를 사용`해서 연관된 객체를 찾습니다.**

---

## 5. 단방향 연관관계

+ **가장 기본이 되는 연관관계**

+ 예제 시나리오
  1. 회원과 팀이 있습니다.
  2. 회원은 하나의 팀에만 소속될 수 있습니다.
  3. 회원과 팀은 다대일 관계입니다.

---

## 5-1. 객체 지향 모델링

+ 단방향 연관관계에서 객체 지향 스럽게 모델링 하는 방법

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-02.png)

+ 여기서 이 둘의 관계가 누가 `1`이고 누가 `N`인지가 정말 중요합니다.
  + **DB관점에서 중요한 것입니다.**
  + 어노테이션도 결국 다 데이터베이스와 매핑한 것들이기 때문에

---

## 5-2. 엔티티 코드

+ **`객체의 참조`와 테이블의 `외래 키를 매핑`합니다.**

```java
@Entity
public class Member {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @Column(name = "USERNAME")
    private String username;

//    @Column(name = "TEAM_ID")
//    private Long teamId;

   /**
     * AFTER
     * 연관관계는
     * TEAM이 1, MEMBER가 N
     * 1. 그러므로 'Member 입장에서는' ManyToOne이라는 어노테이션으로 매핑
     * 2. Team 래퍼런스와 TEAM_ID(FK)랑 매핑해야 합니다. (JoinColumn 사용)
     */
    @ManyToOne  //관계가 무엇인지?
    @JoinColumn(name = "TEAM_ID") //이 관계에서 조인하는 컬럼은 무엇인지?
    private Team team;

    //...
}
```

+ 그러면 이렇게 연관관계가 매핑되는 것입니다.
  + `TEAM`과 `TEAM_ID(FK)`를 연관관계 매핑

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-03.png)

---

## 5-3. USECASE

---

## 5-3-1. 연관 관계 저장

```java
//팀 저장 
Team team = new Team(); 
team.setName("TeamA");  
em.persist(team); 

//회원 저장 
Member member = new Member();  
member.setName("member1"); 
member.setTeam(team); //단방향 연관관계 설정, 참조 저장  
em.persist(member); 
```

---

## 5-3-2. 연관 관계 조회

+ 참조로 연관관계를 조회하므로 `객체 그래프 탐색` 가능

```java
//조회 
Member findMember = em.find(Member.class, member.getId());

//참조를 사용해서 연관관계 조회 
Team findTeam = findMember.getTeam();
```

---

## 5-3-3. 연관관계 수정

+ 예를 들어, TEAM_A => TEAM_B로 바꾸고 싶은 경우

```java
// 새로운 팀B 
Team teamB = new Team(); 
teamB.setName("TeamB");  
em.persist(teamB); 

// 회원1에 새로운 팀B 설정  
member.setTeam(teamB); 
```

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
