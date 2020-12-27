---
title: Spring-Boot) Spring Data JPA 
author: cotchan 
date: 2020-12-28 04:24:21 +0800 
categories: [Spring-Boot, Spring-Boot_JPA]
tags: [spring-boot] 
---

## 1. Spring Data JPA란

+ `Spring Data JPA Framework`는 JPA를 편리하게 사용하도록, Spring이 JPA를 한 번 더 래핑한 기술입니다. 

---

## 2. Spring Data JPA 사용방법

## 2-1. 리포지토리 인터페이스 생성

```java
//SpringDataJpaMemberRepository.java

import hello.hellospring.domain.Member;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.Optional;

public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository {

    @Override
    Optional<Member> findByName(String name);
}
```


---


## 2-2. 스프링 설정 변경

+ SpringDataJpaMemberRepository를 사용하도록 스프링 설정 변경 

```java
//SpringConfig.java

@Configuration
public class SpringConfig {

    private final MemberRepository memberRepository;

    @Autowired
    public SpringConfig(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository);
    }
}
```

---

## 3. 스프링 데이터 JPA 제공 클래스


![Desktop View](/assets/img/post/spring-boot/2020-12-28-springboot-data-jpa.png)


---

## 4. 스프링 데이터 JPA 제공 기능

+ **인터페이스를 통한 기본적인 CRUD**

+ **`findByName()`, `findByEmail()`처럼 메서드 이름만으로 조회 기능 제공**
   
```java
public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository {

    //아래와 같은 식으로 쿼리를 만들어줍니다.
    //select m from Member m where m.name = ?
    @Override
    Optional<Member> findByName(String name);
}
```

+ **페이징 기능 자동 제공**

+ 참고사항
    + 실무에서는 JPA와 스프링 데이터 JPA를 기본으로 사용하고, 복잡한 동적 쿼리는 Querydsl이라는 라이브러리를 사용하면 됩니다. Querydsl을 사용하면 쿼리도 자바 코드로 안전하게 작성할 수 있고, 동적 쿼리도 편리하게 작성할 수 있습니다. 이 조합으로 해결하기 어려운 쿼리는 JPA가 제공하는 네이티브 쿼리를 사용하거나, 앞서 학습한 스프링 JdbcTemplate를 사용하면 됩니다.


---

+ 출처
	+ [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
