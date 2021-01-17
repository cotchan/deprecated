---
title: Spring-Boot) JPA 기본
author: cotchan 
date: 2020-12-23 04:52:21 +0800 
categories: [Spring-Boot, Spring-Boot_JPA]
tags: [spring-boot] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. JPA란

+ JPA는 기존의 반복 코드는 물론이고, 기본적인 SQL도 JPA가 직접 만들어서 실행해줍니다.
+ JPA를 사용하면, SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환을 할 수 있습니다.
+ JPA를 사용하면 개발 생산성을 크게 높일 수 있습니다.


---

## 2. JPA 추가 방법


## 2-1. 라이브러리 추가

+ `build.gradle`에 dependecy 추가

```java
//build.gradle

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
}
```

## 2-2. 스프링 부트에 JPA 설정 추가

+ `resources/application.properties`

```java
spring.datasource.url=jdbc:h2:tcp://localhost/~/test 
spring.datasource.driver-class-name=org.h2.Driver 
spring.datasource.username=sa

spring.jpa.show-sql=true	
spring.jpa.hibernate.ddl-auto=none	
//spring.jpa.hibernate.ddl-auto=create  
```

![Desktop View](/assets/img/post/spring-boot/2020-12-23-springboot-jpa-application-setting.png)

---

## 2-3. JPA 엔티티 매핑

+ `@Entity`
+ `javax.persistence.Entity`
+ **@Entity 어노테이션을 달면 이제부터 이 Entity는 JPA가 관리한다는 의미입니다.**

```java
//Member.java
@Entity
public class Member {
    //...
}
```

+ 그리고 PK를 매핑해줍니다.
    + `@Id`
+ DB에 값을 넣으면 DB가 알아서 ID를 자동으로 생성해주는 것을 `IDENTITY` 전략이라고 합니다.

```java
//Member.java
@Entity
public class Member {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
}
```

+ 만약에 DB 컬럼명이 `username`이라면 아래와 같이 커스텀이 가능합니다.

```java
//Member.java
@Entity
public class Member {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "username")
    private String name;
}
```

---

## 3. JPA로 리포지토리 만들기

```java
//JpaMemberRepository.java
public class JpaMemberRepository implements MemberRepository{

    private final EntityManager em;

    public JpaMemberRepository(EntityManager em) {
        this.em = em;
    }

    //...
```

+ JPA는 `EntityManager`로 모든 게 동작합니다. 
+ **Entity Manager는 스프링부트가 자동으로 생성해줍니다.**
    + build.gradle에 'org.springframework.boot:spring-boot-starter-data-jpa'를 등록하면 됩니다. 
+ JPA를 사용하려면 EntityManager를 주입받아야 합니다.

---

## 4. 서비스 계층에 @Transactional 추가

+ JPA를 사용하려면 서비스 계층에 `@Transactional`을 추가해줘야 합니다.

```java
//MemberService.java
import org.springframework.transaction.annotation.Transactional;

@Transactional
public class MemberService {

}
```

---

## 5. JPA를 사용하도록 스프링 설정 변경

```java
//SpringConfig.java
@Configuration
public class SpringConfig {

    private EntityManager em;

    @Autowired
    public SpringConfig(EntityManager em) {
        this.em = em;
    }

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository() {
        return new JpaMemberRepository(em);
    }
}
```


---

+ 출처
	+ [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
