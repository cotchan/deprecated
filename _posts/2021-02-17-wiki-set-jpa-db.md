---
title: Wiki) JPA와 DB 설정, 동작 확인
author: cotchan 
date: 2021-02-17 20:21:21 +0800 
categories: [Wiki, Wiki_Spring-boot]
tags: [spring-setting] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. application.yml 작성

```yml
spring:
  datasource:
    url: jdbc:h2:tcp://localhost/~/jpashop
    username: sa
    password:
    driver-class-name: org.h2.Driver

  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
        show_sql: true
        format_sql: true

logging:
  level:
    org.hibernate.SQL: debug
```

+ `spring.jpa.hibernate.ddl-auto: create`
    + 이 옵션은 애플리케이션 실행 시점에 테이블을 drop하고, 다시 생성합니다.
+ `show_sql`
    + 이 옵션은 `System.out`에 하이버네이트 실행 SQL을 남깁니다.
+ `org.hibernate.SQL`
    + 이 옵션은 `logger`를 통해 하이버네이트 실행 SQL을 남깁니다.
+ `yml` 파일은 `스페이스 2칸`으로 계층을 만듭니다.

---

## 2. 실제 동작하는지 확인

## 2-1. 도메인 클래스 생성

```java
//Member.java

package jpabook.jpashop;

import lombok.Getter;
import lombok.Setter;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
@Getter @Setter
public class Member {

    @Id @GeneratedValue
    private Long id;
    private String username;

}

```
---

## 2-2. Repository 생성

```java
//MemberRepository.java
package jpabook.jpashop;

import org.springframework.stereotype.Repository;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;

@Repository
public class MemberRepository {

    //Jpa를 쓰기 때문에 EntityManager가 있어야 합니다.
    //Spring-Boot를 쓰기 때문에, 이 어노테이션이 있으면 SpringBoot Container가 매니저를 주입을 해줍니다.
    //implementation 'spring-boot-starter-data-jpa' 때문에 가능
    @PersistenceContext
    private EntityManager em;

    public Long save(Member member) {
        em.persist(member);
        return member.getId();
    }

    public Member find(Long id) {
        return em.find(Member.class, id);
    }
}
```

---

## 3. TestCase 생성

+ `단축키`: `cmd` + `shift` + `T`

```java
//MemberRepositoryTest.java
package jpabook.jpashop;

import org.assertj.core.api.Assertions;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.annotation.Rollback;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.transaction.annotation.Transactional;

import static org.junit.Assert.*;

//JUnit에게 "나 스프링과 관련된 것으로 테스트 할 거야"라고 알려주는 것.
@RunWith(SpringRunner.class)
@SpringBootTest
public class MemberRepositoryTest {

    @Autowired MemberRepository memberRepository;

    @Test
    //import org.springframework.transaction.annotation.Transactional 사용 권장
    //EntityManager를 통한 모든 데이터 변경은 항상 트랜잭션 안에서 이뤄져야 합니다.
    @Transactional
    public void testMember() throws Exception {

        //given
        Member member = new Member();
        member.setUsername("memberA");

        //when
        final Long savedId = memberRepository.save(member);
        final Member findMember = memberRepository.find(savedId);

        //then
        Assertions.assertThat(findMember.getId()).isEqualTo(member.getId());
        Assertions.assertThat(findMember.getUsername()).isEqualTo(member.getUsername());
        Assertions.assertThat(findMember).isEqualTo(member);
    }
}
```



---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
