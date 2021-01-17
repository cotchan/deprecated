---
title: Spring-Boot) Repository 클래스 생성방법(1)
author: cotchan 
date: 2021-01-05 00:00:21 +0800 
categories: [Spring-Boot, Spring-Boot_Repository]
tags: [spring-boot] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. Repository 클래스 생성에 필요한 것

+ **리포지토리 클래스를 생성할 때 아래의 어노테이션이 필요합니다.**

+ case1

```java
@Repository
@RequiredArgsConstructor
public class RepositoryClass {

    private final EntityManager em;
```

---

+ case2

```java
@Repository
public class RepositoryClass {

    //jpa가 제공하는 표준 어노테이션
    //이 어노테이션을 사용하면 여기에 스프링이 생성한 EntityManager를 주입해줍니다.
    @PersistenceContext
    private EntityManager em;
```

---

## 2. 적용된 기술 설명


+ **`@Repository`**
  + ComponentScan의 대상이 되어 빈으로 등록됩니다. @Component 어노테이션을 래핑  
+ **`@RequiredArgsConstructor`**
  + `final` 키워드가 선언 되어 있는 필드만 가지고 생성자를 만들어줍니다.
  + 생성자 주입방식을 사용하기 위해 이렇게 사용합니다.
  + 스프링은 생성자가 1개인 경우 별도의 어노테이션이 없어도 자동으로 @Autowired 옵션을 적용해줍니다.

+ EntityManager에 대해 `case2 방식`을 사용하는 게 `기본`입니다.
+ 하지만 `case1`처럼 해도 스프링이 EntityManager를 주입시켜줍니다. 

---

## 3. Sample Code

+ Sample1: `MemberRepository`

```java
package jpabook.jpashop.repository;

import jpabook.jpashop.domain.Member;
import org.springframework.stereotype.Repository;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import java.util.List;

//ComponentScan에 의해 자동으로 스프링 빈으로 등록됩니다.
@Repository
public class MemberRepository {

    //jpa가 제공하는 표준 어노테이션
    //이 어노테이션을 사용하면 여기에 스프링이 생성한 EntityManager를 주입해줍니다.
    @PersistenceContext
    private EntityManager em;

//    public MemberRepository(EntityManager em) {
//        this.em = em;
//    }

    //저장
    //persist하면 영속성 컨텍스트에 멤버 객체를 넣음
    //트랜잭션이 커밋 되는 시점에 db에 반영
    public void save(Member member) {
        //persist를 하면 영속성 컨텍스트에 올라가는데, 이 때 Entity의 key(key, value에서)는 PK가 됩니다.
        //즉, Entity의 id값이 항상 생성되어있는 게 보장이 됩니다.
        em.persist(member);
    }

    //조회 (1개)
    public Member findOne(Long id) {
        //arg[0]:type, arg[1]: PK
        return em.find(Member.class, id);
    }

    //리스트 조회
    public List<Member> findAll() {
        //arg[0]: JPQL query, arg[1]: 반환 타입
        return em.createQuery("select m from Member m", Member.class)
                .getResultList();
    }

    public List<Member> findByName(String name) {
        //parameter binding
        return em.createQuery("select m from Member m where m.name = :name", Member.class)
                .setParameter("name", name)
                .getResultList();
    }
}
```

+ Sample2: `OrderRepository`

```java
package jpabook.jpashop.repository;

import jpabook.jpashop.domain.Order;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Repository;

import javax.persistence.EntityManager;
import java.util.List;

@Repository
@RequiredArgsConstructor
public class OrderRepository {

    private final EntityManager em;

    public void save(Order order) {
        em.persist(order);
    }

    public Order findOne(Long id) {
        return em.find(Order.class, id);
    }

//    public List<Order> findAll(OrderSearch orderSearch) {}
}
```


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
