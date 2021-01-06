---
title: HelloShop) 회원 도메인 개발(Entity, Repo, Service, Test) 
author: cotchan
date: 2021-01-07 08:32:21 +0800
categories: [Project, HelloShop]
tags: # [TAG]     # TAG names should always be lowercase
---

## 1. 회원 Entity 개발

![Desktop View](/assets/img/post/helloShop/2021-01-06-analysis-design2.png)

```java
package jpabook.jpashop.domain;

import lombok.Getter;
import lombok.Setter;

import javax.persistence.*;
import java.util.ArrayList;
import java.util.List;

@Entity
@Getter @Setter
public class Member {

    @Id @GeneratedValue
    @Column(name = "member_id")
    private Long id;

    private String name;

    @Embedded
    private Address address;

    //Member 입장에서는 Member와 Order는 일대다 관계
    //"나는 연관관계의 주인이 아닙니다"를 표시 해줌(mappedBy)
    //"나는 Order 테이블에 있는 member 필드에 의해서 매핑된 겁니다."라고 하는 것. (즉, 읽기 전용 이라는 것)
    @OneToMany(mappedBy = "member")
    private List<Order> orders = new ArrayList<>();
}
```

---

## 2. 회원 리포지토리 개발

## 2-1. 리포지토리 주요 기술

+ **`@Repository`**
    + 스프링 빈으로 등록. JPA 예외를 스프링 기반 예외로 예외 변환
    + ComponentScan에 의해 자동으로 스프링빈으로 등록됩니다.
+ **`@PersistenceContext`**
    + 엔티티 매니저(EntityManager) 주입
+ **`@PersistenceUnit`** 
    + 엔티티 매니저 팩토리(EntityManagerFactory) 주입

+ **기능 설명**
    + `save()`
    + `findOne()`
    + `findAll()`
    + `findByName()`

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
    //이 어노테이션을 사용하면
    //여기에 스프링이 생성한 EntityManager를 주입해줍니다.
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

---

## 3. 회원 서비스 개발

## 3-1. 서비스 주요 기술

+ **`@Service`**
+ **`@Transactional`**
    + 트랜잭션, 영속성 컨텍스트 관련
    + JPA의 모든 데이터 변경, 로직들은 전부 트랜잭션 안에서 실행되어야 합니다.
    + `readOnly = true`
        + **데이터의 변경이 없는 읽기 전용 메서드에 사용합니다.**
        + 영속성 컨텍스트를 플러시 하지 않으므로 약간의 성능 향상(읽기 전용에는 다 적용시키기) 
+ **`@Autowired`**
    + 생성자 injection에서 많이 사용, 생성자가 하나인 경우 생략 가능합니다.

+ **기능 설명**
    + `join()`
    + `findMembers()`
    + `findOne()`

+ **참고사항**
    + 실무에서는 아래 코드의 `validateDuplicateMember`처럼 검증 로직이 있다고 하더라도 멀티 쓰레드 상황을 고려하여 회원 테이블의 `회원명 컬럼에` `유니크 제약 조건`을 추가하는 것이 안전합니다. 

```java
package jpabook.jpashop.service;

import jpabook.jpashop.domain.Member;
import jpabook.jpashop.repository.MemberRepository;
import lombok.AllArgsConstructor;
import lombok.RequiredArgsConstructor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Service
//JPA의 모든 데이터 변경, 로직들은 전부 트랜잭션 안에서 실행되어야 합니다.
@Transactional(readOnly = true)
//@AllArgsConstructor //모든 필드를 가지고 생성자를 만들어줍니다.
@RequiredArgsConstructor    //final이 있는 필드만 가지고 생성자를 만들어줍니다.
public class MemberService {

    private final MemberRepository memberRepository;

//    @AllArgsConstructor, @RequiredArgsConstructor가 아래와 같은 생성자 생성
//    @Autowired
//    public MemberService(MemberRepository memberRepository) {
//        this.memberRepository = memberRepository;
//    }

    /**
     * 회원 가입
     */
    @Transactional  //write에는 readOnly = false가 적용되게 합니다.
    public Long join(Member member) {

        //중복회원 검증 로직
        validateDuplicateMember(member);
        memberRepository.save(member);
        return member.getId();
    }

    /**
     * 중복회원 검증 로직
     */
    private void validateDuplicateMember(Member member) throws IllegalStateException {
        //EXCEPTION
        List<Member> findMembers = memberRepository.findByName(member.getName());
        if (!findMembers.isEmpty())
        {
            throw new IllegalStateException("이미 존재하는 회원입니다.");
        }
    }

    //회원 전체 조회
    public List<Member> findMembers() {
        return memberRepository.findAll();
    }

    //한 건 조회
    public Member findOne(Long memberId) {
        return memberRepository.findOne(memberId);
    }
}
```


---

## 4. 회원 기능 테스트



---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)


