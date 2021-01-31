---
title: JPQL) 벌크 연산
author: cotchan 
date: 2021-01-31 23:24:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 벌크 연산이란

+ 우리가 아는 일반적인 SQL의 UPDATE문과 DELETE문을 의미합니다.
  + PK를 찍어서 한 건을 업데이트하거나 지우는 것을 제외한 나머지 모든 UPDATE, DELETE문

+ 예제
  + 재고가 10개 미만인 모든 상품의 가격을 10% 상승하려면?
  + JPA 변경 감지 기능으로 실행하려면 너무 많은 SQL을 실행해야 합니다.
    1. 재고가 10개 미만인 상품을 리스트로 조회합니다.
    2. 상품 엔티티의 가격을 10% 증가시킵니다.
    3. 트랜잭션 커밋 시점에 변경감지가 동작합니다.
  + 변경된 데이터가 100건이라면 100번의 UPDATE SQL이 실행됩니다.

---

## 2. 벌크 연산 예제

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-final-01.png)

```java
//example. 모든 회원의 나이를 20살로 변경

//이 시점에 Flush 자동 호출됩니다.
//Flush는 커밋을 하거나, 쿼리가 나갈 때 자동으로 호출됩니다. 또는 강제로 호출할 때
int resultCount = em.createQuery("update Member m set m.age = 20")
    .executeUpdate();

System.out.println("resultCount = " + resultCount);
```

---

## 3. 벌크 연산 주의점

+ 벌크 연산은 영속성 컨텍스트를 무시하고 데이터베이스에 직접 쿼리를 날립니다.

+ 해결책(2가지)
  1. 벌크 연산을 실행합니다.
    + 영속성 컨텍스트에 작업을 아무것도 하지 말고 벌크 연산을 실행합니다.
    + 영속성 컨텍스트에 아무 것도 없기 때문에 문제가 되지 않습니다.
  2. **벌크 연산을 실행한 다음에 영속성 컨텍스트를 초기화합니다.**


```java
//아래처럼 벌크 연산 수행 후 꼭 영속성 컨텍스트를 초기화 해줍니다.

//모든 회원의 나이를 20살로 변경
int resultCount = em.createQuery("update Member m set m.age = 20")
    .executeUpdate();
            
em.clear();

Member findMember = em.find(Member.class, member1.getId());
System.out.println("resultCount = " + findMember.getAge());
```

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
