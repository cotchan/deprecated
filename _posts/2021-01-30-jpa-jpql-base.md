---
title: JPQL) 기본 문법(+ 기능)
author: cotchan 
date: 2021-01-30 23:00:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. JPQL이란

+ JPQL은 객체지향 쿼리 언어입니다. 
+ 따라서 테이블을 대상으로 쿼리하는 것이 아니라 **엔티티 객체를 대상으로 쿼리**합니다.
+ JPQL은 SQL을 추상화해서 특정데이터베이스 SQL에 의존하지 않습니다.
+ **JPQL은 결국 SQL로 변환됩니다.**

---

## 2. 예제 모델

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-jpql-base-01.png)

---

## 3. JPQL 문법

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-jpql-base-02.png)

---

```java
//example
select m from Member as m where m.age > 18
```

+ 차이점 
  + **Member는 Entity입니다.**
  + 엔티티와 속성은 대소문자를 구분해서 써야합니다.(Member, age)
  + JPQL 키워드는 대소문자를 구분하지 않습니다.
  + 엔티티 이름 사용, 테이블 이름 아님(Member)
    + 여기서 말하는 엔티티 이름은 @Entity(name = "???")을 의미합니다.
    + 보통 클래스명과 동일하게 맞춰집니다.
  + **별칭은 필수(m) (as는 생략 가능)**

---

## 3-1. 집합과 정렬

+ 모두 사용가능합니다.
  + `GROUP BY`
  + `HAVING`
  + `ORDER BY`

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-jpql-base-03.png)

---

## 3-2. TypeQuery, Query

+ TypeQuery: 반환 타입이 명확할 때 사용합니다.
+ Query: 반환 타입이 명확하지 않을 때 사용합니다.

```java
//두 번째 parameter로 응답 클래스에 대한 type 값을 줄 수 있습니다.
//기본적으로 Entity 값을 줍니다.
final TypedQuery<Member> query1 = em.createQuery("select m from Member m", Member.class);
            
//아래 쿼리의 경우에는 username은 String, age는 int이므로 타입을 특정할 수 없습니다. 
final Query query2 = em.createQuery("select m.username, m.age from Member m");
```

---

## 3-3. 결과 조회

+ **getResultList()는 결과가 없으면 빈 리스트를 반환합니다.**
  + NullpointerException 걱정을 하지 않아도 됩니다.
+ **getSingleResult()는 결과가 정확하게 하나가 나와야 됩니다.**
  + 그 외의 경우에는 Exception이 터집니다.

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-jpql-base-04.png)

---

## 3-4. 파라미터 바인딩

+ **`이름 기준`, `위치 기준`으로 바인딩할 수 있습니다.**

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-jpql-base-05.png)

---

+ **이름 기준 예시**

```java
TypedQuery<Member> query = em.createQuery("select m from Member m where m.username = :username", Member.class);
query.setParameter("username", "member1");
Member singleResult = query.getSingleResult();
System.out.println(singleResult);
```

+ **그런데 보통은 `메서드 체이닝`으로 아래와 같이 사용합니다.**

```java
Member singleResult = em.createQuery("select m from Member m where m.username = :username", Member.class)
    .setParameter("username", "member1")
    .getSingleResult();

System.out.println(singleResult.getUsername());
```

---

+ 위치 기반 바인딩은 가급적 안 쓰는 게 좋습니다.
  + 다른 parameter가 들어오게 되면 위치가 바뀌게 되므로 잠재적 장애요인이 됩니다.
  + 이름 기준처럼 "이름"을 기준으로 하면 다른 parameter가 들어와도 엮이지 않습니다.

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
