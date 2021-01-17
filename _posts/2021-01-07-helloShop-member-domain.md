---
title: HelloShop) 회원 도메인 개발(Entity, Repo, Service, Test) 
author: cotchan
date: 2021-01-07 08:32:21 +0800
categories: [Project, HelloShop]
tags: # [TAG]     # TAG names should always be lowercase
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

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

+ **테스트 요구사항**
	+ 회원가입을 성공해야 한다.
	+ 회원가입 할 때 같은 이름이 있으면 예외가 발생해야 합니다.

---

## 4-1. 테스트 관련 주요 기술

+ **`@RunWith(SpringRunner.class)`**
	+ 스프링과 테스트 통합
+ **`@SpringBootTest`**
	+ 스프링 부트를 띄우고 테스트(이게 없으면 `@Autowired`도 실패합니다)
+ **`@Transactional`**
	+ 반복 가능한 테스트 지원
	+ **각각의 테스트를 실행할 때마다 트랜잭션을 시작하고, `테스트가 끝나면 트랜잭션을 강제로 롤백합니다.`**
	+ 이 어노테이션이 테스트 케이스에서 사용될 때만 롤백합니다.

```java
package jpabook.jpashop.service;

import jpabook.jpashop.domain.Member;
import jpabook.jpashop.repository.MemberRepository;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.annotation.Rollback;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.transaction.annotation.Transactional;

import javax.persistence.EntityManager;

import static org.junit.Assert.*;

//테스트를 실행하려면아래 두개의 어노테이션이 있어야 완전 SpringBoot와 Integration 되어 테스트 가능
@RunWith(SpringRunner.class)    //JUnit 실행할 때 Spring과 엮어서 실행한다는 의미
@SpringBootTest //SpringBoot를 띄운상태로 테스트할 때 필요한 것. 이게 없으면 Autowired도 실패
@Transactional  //Transaction을 커밋을 안하고 롤백을 합니다.
public class MemberServiceTest {

    @Autowired
    MemberService memberService;

    @Autowired
    MemberRepository memberRepository;

    @Test
    public void 회원가입() throws Exception {
        //given
        Member member = new Member();
        member.setName("kim");

        //when
        final Long saveId = memberService.join(member);

        //then
        assertEquals(member, memberRepository.findOne(saveId));
    }

    //validateDuplicateMember 검증
    //여기서 예외가 터져서 잡히는 Exception이 IllegalStateException이면 됩니다.
    //이 코드가 있기에 try-catch 생략 가능!
    @Test(expected = IllegalStateException.class)
    public void 중복_회원_예외() throws Exception {
        //given
        Member member1 = new Member();
        member1.setName("kim");

        Member member2 = new Member();
        member2.setName("kim");

        //when
        memberService.join(member1);
        memberService.join(member2);    //예외가 발생해야 합니다.

        //then
        //위 문장에서 예외가 나가지 않고  여기까지 코드가 나온다면
        fail("예외가 발생해야 합니다");
    }
}
```

---

## 4-2. 테스트 케이스를 위한 설정

+ **테스트 켸이스는 격리된 환경에서 실행하고 끝나면 데이터를 초기화하는 것이 좋습니다.**
	+ 그런면에서 `Memory DB`를 사용하는 것이 가장 이상적입니다.
+ **추가적으로 테스트 케이스를 위한 스프링 환경과, 일반적으로 애플리케이션을 실행하는 환경은 보통다르므로 설정 파일을 다르게 사용하는 것이 바람직합니다.**
+ 아래와 같이 간단하게 테스트용 설정 파일을 추가해줍니다.

```java
// test/resources/application.yml
spring:

logging:
  level:
    org.hibernate.SQL: debug
``` 

+ **이제 테스트에서 스프링을 실행하면 이 위치(`test/resources`)에 있는 설정 파일을 읽습니다.**
	+ 만약 이 위치에 없으면 `src/resources/application.yml`을 읽습니다.

+ **스프링부트는 `datasource` 설정이 없으면 기본적으로 메모리 DB를 사용하고, driver-class도 현재 등록된 라이브러리를 보고 찾아줍니다.**
	+ 추가로 `ddl-auto`도 `create-drop` 모드로 동작합니다.
	+ create-drop는 내가 가지고 있는 Entity를 전부 drop한 다음에 Create를 하고 Application 실행 + Application 종료시점에 drop 실행

+ 따라서 데이터소스나, JPA 관련 별도의 추가 설정을 하지 않아도 됩니다.


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)


