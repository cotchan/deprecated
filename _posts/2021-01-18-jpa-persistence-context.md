---
title: JPA) 영속성 컨텍스트
author: cotchan 
date: 2021-01-18 00:00:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-1.png)

## INTRO

EntityManagerFactory는 고객의 요청이 올 때 마다 EntityManager를 생성합니다.    
이 EntityManager는 내부적으로 DB의 Connection을 사용해서 DB를 사용하게 됩니다.    

---

## 1. 영속성 컨텍스트

+ **"엔티티를 영구 저장하는 환경"이라는 뜻입니다.**
+ **`EntityManager.persist(entity);`** 
  + 위 메서드는 DB에 저장하는 게 아니라, 영속성 컨텍스트를 통해서 Entity를 영속화한다는 뜻입니다.
  + persist 메서드는 엔티티를 영속성 컨텍스트라는 곳에 저장한다는 뜻입니다.

+ **엔티티 매니저를 통해서 영속성 컨텍스트에 접근합니다.**


---

## 2. 엔티티의 생명주기

+ 엔티티는 다음과 같은 생명주기를 가지고 있습니다.

+ **비영속(new/transient)**
  + 최초의 객체를 생성한 상태
  + 영속성 컨텍스트와 전혀 관계가 없는 `새로운` 상태입니다.

+ **영속(managed)**
  + 비영속 상태에서 `persist`를 하면 영속상태가 됩니다.
  + 영속성 컨텍스트에 `관리`되는 상태입니다.

+ **준영속(detached)**
  + 영속성 컨텍스트에 저장되었다가 `분리`된 상태입니다.

+ **삭제(removed)**
  + `삭제`된 상태입니다.

---

## 2-1. 비영속 상태

+ 비영속 상태란 멤버 객체를 생성을 하고, 그리고 EntityManager에 아무것도 안하고 생성만 한 상태
+ 즉 JPA와 전혀 관계가 없는 상태

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-1.png)

---

## 2-2. 영속 상태

+ **EntityManager안에는 영속성 컨텍스트라는 게 있습니다.**
1. 이렇게 멤버 객체를 생성한 다음에 EM를 얻어와서 EM에 persist를 해서 멤버 객체를 집어넣으면
2. EntityManager 안에있는 영속성 컨텍스트안에 멤버 객체가 들어가면서 영속상태가 됩니다.
3. 즉, EM 안에 있는 영속성 컨텍스트를 통해서 이 멤버가 관리된다는 뜻입니다.(이 때 DB에 저장되지 않습니다.)
4. 쿼리가 날아가는 시점은 트랜잭션을 커밋하는 시점에 영속성 컨텍스트에 있는 애가 DB에 쿼리가 날아갑니다.

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-2.png)

---

## 2-3. 준영속, 삭제 상태

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-3.png)

---

## 3. 영속성 컨텍스트의 이점

+ 쉽게 말해서 Application과 Database 사이에 중간 계층이 있는 것입니다.
  + **1차 캐시**
  + **동일성(identity) 보장**
  + **트랜잭션을 지원하는 쓰기 지연**
  + **변경 감지(Dirty Checking)**
  + **지연 로딩(Lazy Loading)**

---


## 3-1. 1차 캐시

+ **영속성 컨텍스트는 내부에 1차 캐시를 들고 있습니다.**
+ 영속 상태가 되면 무슨 일이 발생하냐면 
  + `@Id` - DB PK로 매핑한 애가 KEY가 됩니다.
  + `Entity` - Entity객체 자체가 값이 됩니다.

+ key  = "member1"
+ 값 = member 객체 자체가 값이 됩니다.

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-4.png)

---

+ 이렇게 되면 무슨 이점? 
  + 조회할 때 내가 이렇게 저장을 해놓고 조회를 하면, JPA는 find로 조회를 하면서 어떤 시도를 하냐면
  + 영속성 컨텍스트에서 1차 캐시를 먼저 뒤집니다. (DB를 뒤지는 게 아님)
  + 그래서 값이 있네? 그러면 그냥 캐시에 있는 값을 그냥 조회해옵니다.

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-5.png)

---

+ BUT 1차 캐시에 없는 경우?
  + find로 찾음
  + 1차 캐시에 없음
  + DB에서 조회를 합니다.
  + DB에서 조회한 member2를 1차 캐시에 저장합니다.
  + 그리고 member2를 반환합니다.

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-6.png)

---

+ EM은 데이터베이스 트랜잭션 단위로 보통 만들고, 트랜잭션이 종료될 때 EM을 종료시켜버립니다.
+ 즉, 고객의 요청이 들어와서 비즈니스 로직이 끝나면 영속성 컨텍스트를 지워버립니다.
+ 고객 여러명이 사용하며 공유하는 캐시는 아닙니다.

---

## 3-2. 영속 엔티티의 동일성 보장

+ **단, 같은 트랜잭션 안에서 실행을 해야 합니다.**
+ JPA는 마치 자바 컬렉션에서 가져오듯이 영속 엔티티의 동일성을 보장해줍니다. == 비교를 보장
+ 이게 가능한 이유는 위에 말한 1차 캐시에서 가져오기 때문에 가능합니다.

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-7.png)

---

## 3-3. 엔티티 등록시 쓰기 지연 가능

+ **커밋하는 순간 DB에 INSERT SQL을 보냅니다.**

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-8.png)

---

## 3-3-1. 쓰기 지연 내부 동작

1. 순차적으로 넣었을 때, 영속성 컨텍스트안에는 1차 캐시 외에도 쓰기 지연 SQL 저장소라는 게 있습니다. 
2. 그래서 memberA를 persist로 넣으면 memberA가 1차 캐시에 들어가면서, JPA가 이 엔티티를 분석해서 INSERT 쿼리를 생성합니다.
3. 그래서 쓰기 지연 SQL 저장소에 쌓아놓습니다.
4. 그리고 memberB를 persist 하면, 이 때도 memberB를 1차 캐시에 넣습니다. 
5. 그리고 INSERT SQL을 생성해서 쓰기 지연 SQL 저장소에 차곡차곡 쌓습니다.
6. 현재 A,B 둘 다 쌓여있습니다.
7. 그리고 트랜잭션을 커밋하는 시점에 쓰기 지연 SQL저장소에 있던 애들이 플러쉬가 되면서 DB로 날아갑니다.
8. 그리고 실제 데이터베이스로 트랜잭션이 커밋됩니다. 이 메커니즘으로 돌게 됩니다.

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-9.png)

---

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-10.png)

---

+ 쓰기 지연을 사용하면 버퍼링 기능을 사용할 수 있습니다.
  + 버퍼링 기능이란? 모았다가 한 번에 DB로 보내는 기술

---

+ **`엔티티 삭제`도 위에 언급한 쓰기 지연 메커니즘과 동일합니다.**
+  트랜잭션 커밋 시점에 delete 쿼리가 나갑니다.

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-13.png)

---

## 3-4. 엔티티 수정 (변경 감지)

+ **Java Collection을 다루듯이 변경을 다룰 수 있게 해줍니다.**

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-11.png)

---

## 3-4-1. 동작 매커니즘

1. JPA는 DB를 commit하는 시점에, 커밋을 하면 내부적으로 flush가 호출이 됩니다. 
2. Entity와 스냅샷을 비교합니다.
3. 1차 캐시안에는 @Id(PK), Entity, 스냅샷이 있습니다. 스냅샷이라는 건 내가 영속성 컨텍스트에서 값을 읽어온 최초 시점의 상태를 스냅샷으로 가지고 있습니다.
4. JPA가 트랜잭션이 커밋되는 시점에 내부적으로 flush가 호출되면서, JPA가 Entity와 스냅샷을 일일이 비교합니다.
5. 비교를 해보고 바뀐 게 있다면, UPDATE 쿼리를 쓰기 지연 SQL 저장소에 만들어두고 DB에 반영을 합니다. 그리고 커밋합니다.

![Desktop View](/assets/img/post/jpa/2021-01-18-jpa-persist-context-12.png)


---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
    + [실전! 스프링 부트와 JPA 활용2 - API 개발과 성능 최적화](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-API%EA%B0%9C%EB%B0%9C-%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94/dashboard)
