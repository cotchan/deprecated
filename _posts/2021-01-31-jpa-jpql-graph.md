---
title: JPQL) 경로 표현식
author: cotchan 
date: 2021-01-31 20:30:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

+ **결론**
  + **항상 명시적 조인을 사용하세요!**

---

## 1. 경로 표현식

+ **.(점)을 찍어 객체 그래프를 탐색하는 것**을 의미합니다.
+ 아래와 같이 세 가지 경로 표현식이 있습니다.
  + 어디 필드로 가느냐에 따라서 결과가 달라지게 됩니다. 그러므로 이 세 가지를 구분하는 것이 중요합니다.

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-graph-01.png)

+ `상태 필드`: 직접 값을 찍는 것
+ m.team t: 엔티티를 직접 찍은 것
  + `단일 값 연관 필드`라고 합니다.
+ m.orders o: Member와 orders는 컬렉션으로 엮여있습니다.
  + 이렇게 컬렉션으로 가는 것을 `컬렉션 값 연관 필드`라고합니다.

---

## 2. 경로 표현식 용어 정리

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-graph-02.png)

---

## 3. 경로 표현식 특징

+ **중요**
  + **실무에서 권장하는 방식은 아예 `묵시적 조인을 사용하지 않는 것`입니다.**
  + 명시적 조인을 써야 쿼리를 튜닝하기 수월합니다.

---

## 3-1. 상태 필드

+ **`상태 필드`는 경로 탐색의 끝입니다.** 그러므로 더 이상 탐색이 안 됩니다.
  + 즉, 상태 필드에서 또 .(점)을 찍어서 어디로 타고 갈 수 있는 게 아닙니다.

+ 상태 필드 경로탐색은 SQL과 동일하게 나갑니다.

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-graph-03.png)

---

## 3-2. 단일 값 연관 경로

+ **Member => Team으로 가게 되면 `묵시적으로 내부 조인`이 발생합니다.**
+ **또한 `추가적인 탐색이 가능`합니다.(Team에서 . 점찍고 더 고고싱)**
+ **실무에서는 묵시적인 내부조인이 발생하도록 쿼리를 짜서는 안 됩니다.**

```java
//멤버와 '연관된' 소속된 팀을 가져오겠다는 의미
//객체야 m.team으로 가져오지만 실제로 TABLE이 동작할때는 조인이 발생합니다.
String query = "select m.team From Member m";

List<Team> result = em.createQuery(query, Team.class)
    .getResultList();
```

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-graph-04.png)

---

```java
//실행된 SQL
//member와 member을 조인하고, team을 proejection에 나열합니다.
select
    team1_.TEAM_ID as TEAM_ID1_3_,
    team1_.name as name2_3_ 
from
    Member member0_ 
inner join
    Team team1_ 
        on member0_.TEAM_ID=team1_.TEAM_ID
```

---

## 3-3. 컬렉션 값 연관 경로

+ **묵시적 내부 조인이 발생합니다.**
+ **단, `추가적인 탐색이 불가능`합니다.**

```java
//컬렉션 값 연관 경로로 가져오면 여기서 더 이상 탐색이 불가능합니다!
String query = "select t.members From Team t";
```

---

+ 추가적인 탐색을 하고 싶다면 **FROM 절에서 `명시적 조인을 통해 별칭`을 얻으면 됩니다.**
  + 별칭을 통해 탐색 가능

```java
//t.members m 으로 별칭을 얻은 모습입니다.
String query = "select m.username From Team t join t.members m";
```

---

## 4. 명시적 조인, 묵시적 조인

+ 명시적 조인: join 키워드 직접 사용하는 것을 의미합니다.
  + select m from Member m **join m.team t**

+ 묵시적 조인: 경로 표현식에 의해 묵시적으로 SQL 조인이 발생합니다.(내부 조인만 가능)
  + select **m.team** from Member m

---

## 5. 경로 표현식 - 예제

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-graph-05.png)

---

## 6. 묵시적 조인 시 주의사항

+ 경로 탐색을 사용한 묵시적 조인 시 주의사항
  + 항상 내부 조인입니다.
  + 컬렉션은 경로 탐색의 끝입니다. 더 탐색하고 싶다면 명시적 조인을 통해 별칭을 얻어야 합니다.
  + 경로 탐색은 주로 SELECT, WHERE 절에서 사용하지만 묵시적 조인으로 인해 SQL의 FROM(JOIN)절에 영향을 줍니다.

---

## 7. 실무 조언

+ **묵시적 조인을 사용하지 않습니다. 대신에 명시적 조인을 사용합니다.**
+ 조인은 SQL 튜닝에 중요한 포인트입니다.
+ 묵시적 조인은 조인이 일어나는 상황을 한눈에 파악하기 어렵습니다.

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
