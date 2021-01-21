---
title: JPA_SUMMARY) 연관관계 매핑1. 단방향 연관 관계
author: cotchan 
date: 2021-01-21 08:30:21 +0800 
categories: [JPA, JPA_Summary]
tags: [jpa_summary] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 테이블과 객체의 큰 차이점

+ 객체를 테이블에 맞추어 데이터 중심으로 모델링하면, 협력관계를 만들 수 없습니다.
+ **테이블은 `외래 키로 조인`을 사용해서 연관된 테이블을 찾습니다.**
+ **객체는 `참조를 사용`해서 연관된 객체를 찾습니다.**
+ 객체를 테이블에 맞추어 데이터 중심으로 모델링을 하면, **`협력관계`를 만들 수 없습니다.**

---

## 2. 단방향 연관관계

+ **가장 기본이 되는 연관관계**

+ 예제 시나리오
  1. 회원과 팀이 있습니다.
  2. 회원은 하나의 팀에만 소속될 수 있습니다.
  3. 회원과 팀은 다대일 관계입니다.

---

## 2-1. 객체 지향 모델링

+ 단방향 연관관계에서 객체 지향 스럽게 모델링 하는 방법

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-02.png)

+ 여기서 이 둘의 관계가 누가 `1`이고 누가 `N`인지가 정말 중요합니다.
  + **DB관점에서 중요한 것입니다.**
  + 어노테이션도 결국 다 데이터베이스와 매핑한 것들이기 때문에

---

## 2-2. 엔티티 코드

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

## 2-3. USECASE

---

## 2-3-1. 연관 관계 저장

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

## 2-3-2. 연관 관계 조회

+ 참조로 연관관계를 조회하므로 `객체 그래프 탐색` 가능

```java
//조회 
Member findMember = em.find(Member.class, member.getId());

//참조를 사용해서 연관관계 조회 
Team findTeam = findMember.getTeam();
```

---

## 2-3-3. 연관관계 수정

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
