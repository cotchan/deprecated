---
title: Spring-Boot) 테스트 케이스 작성 기본 개념(2편)
author: cotchan 
date: 2020-12-14 00:50:21 +0800 
categories: [Spring-Boot, Spring-Boot_Test]
tags: [spring-boot] 
---

+ 아래 예제를 바탕으로 작성한 글입니다.
    + [HelloShop) 회원 도메인 개발(Entity, Repo, Service, Test)](https://cotchan.github.io/posts/helloShop-member-domain/)

## 1. Sample Test Class Code

+ 테스트 요구사항
    1. 회원가입을 성공해야 합니다. 
    2. 회원가입을 할 때 같은 이름이 있으면 예외가 발생해야 합니다.

```java
//MemberServiceTest.java
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

    @Autowired
    EntityManager em;

    @Test
    public void 회원가입() throws Exception {
        //given
        Member member = new Member();
        member.setName("kim");

        //when
        final Long saveId = memberService.join(member);

        //then
        //플러시는 영속성 컨텍스트에 있는 내용을 DB에 반영하는 것
        //DB에 일단 쿼리를 날리게 된다(commit)
        em.flush();
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

## 2. 기술 설명

+ **`@RunWith(SpringRunner.class)`**  
    + JUnit을 실행할 때 Spring과 엮어서(통합해서) 실행한다는 의미입니다.
+ **`@SpringBootTest`** 
    + SpringBoot를 띄운 상태로 테스트를 합니다.(이게 없으면 Autowired도 실패합니다.)
+ **`@Transactional`** 
    + **Transaction을 커밋을 안하고 롤백을 합니다.**
    + 반복 가능한 테스트를 지원합니다. 
    + 각각의 테스트를 실행할 때마다 트랜잭션을 시작하고 **테스트가 끝나면 `트랜잭션을` 강제로 `롤백`합니다.**
        + 이 어노테이션이 테스트 케이스에서 사용될 때만 롤백합니다.

---

## 3. 테스트 케이스를 위한 설정

+ 테스트 케이스는 격리된 환경에서 실행하고, 끝나면 데이터를 초기화하는 것이 좋습니다.
+ **그런 면에서 `메모리 DB`를 사용하는 것이 가장 이상적입니다.**  
    + 참고: [H2 Database Engine Cheat Sheet](http://h2database.com/html/cheatSheet.html)

+ **또한 설정 파일(`application.yml`)을 다르게 사용하는 것이 좋습니다.**
    + 테스트 케이스를 위한 스프링 환경과, 애플리케이션을 실행하는 환경은 다르므로

+ **그래서 `test/resources` 경로에 설정파일을 추가하면 테스트용 설정 파일을 사용할 수 있습니다.**
    + 테스트에서 스프링을 실행하면 이 위치에 있는 설정 파일을 읽습니다.
    + 이 위치(test/resources)에 없으면 (src/resources/application.yml)을 읽습니다.

+ 예시1
    + `test/resources/application.yml`

```java
spring:
  datasource:
    // 이 url을 memoryDB로 바꿔주면 됩니다. 메모리 모드로 동작합니다.
    url: jdbc:h2:mem:test
    username: sa
    password:
    driver-class-name: org.h2.Driver

  jpa:
    hibernate:
      ddl-auto: create
 // create는 내가 가지고 있는 Entity를 전부 drop한 다음에 Create를 하고 Application 실행
 // create-drop는 내가 가지고 있는 Entity를 전부 drop한 다음에 Create를 하고 Application 실행 + Application 종료시점에 drop 실행
    properties:
      hibernate:
        show_sql: true
        format_sql: true

  // 스프링부트가 별도의 설정이 없으면 그냥 MemoryDB로 돌려버립니다.

logging:
  level:
    org.hibernate.SQL: debug
    org.hibernate.type: trace
```

+ 예시2
    + `test/resources/application.yml`

+ 스프링부트에 별도의 설정이 없으면 그냥 MemoryDB로 돌려버립니다. 
+ **그래서 아래와 같이 작성해도 Memory DB로 똑같이 동작합니다.**

```java
spring:

logging.level:
    org.hibernate.SQL: debug
```

+ 스프링 부트는 datasource 설정이 없으면, 기본적으로 메모리 DB를 사용합니다. 
+ 또한 `driver-class`도 현재 등록된 라이브러리를 보고 찾아줍니다.
    + ex. `runtimeOnly 'com.h2database:h2'`
+ `ddl-auto`도 `create-drop` 모드로 동작합니다.
+ **따라서 데이터소스나, JPA 관련된 별도의 추가 설정을 하지 않아도 됩니다.**


---

## 3.

---


## 4. 


---


![Desktop View](/assets/img/post/spring-boot/2020-12-10-spring-boot-how-to-build.png){: width="350" class="normal"}







---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
