---
title: Spring-Boot) Spring AOP 기본
author: cotchan 
date: 2020-12-28 05:15:21 +0800 
categories: [Spring-Boot, Spring-Boot_Basic]
tags: [spring-boot] 
---

## 1. AOP가 필요한 상황 

+ 모든 메소드의 호출 시간을 측정하고 싶다면?

+ 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern)

+ 회원 가입 시간, 회원 조회 시간을 측정하고 싶다면?

![Desktop View](/assets/img/post/spring-boot/2020-12-28-springboot-aop1.png)

+ example) MemberService에 회원 조회 시간 측정 로직 추가

```java
package hello.hellospring.service;

@Transactional
public class MemberService {
    /**
    * 회원가입
    */
    public Long join(Member member) {
        long start = System.currentTimeMillis();

        try {
            validateDuplicateMember(member); //중복 회원 검증
            memberRepository.save(member);
            return member.getId(); 
        } finally {
            long finish = System.currentTimeMillis(); 
            long timeMs = finish - start; 
            System.out.println("join " + timeMs + "ms");
        } 
    }    

    /**
    *전체 회원 조회
    */
    public List<Member> findMembers() {
        long start = System.currentTimeMillis();
        try {
            return memberRepository.findAll();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start; 
            System.out.println("findMembers " + timeMs + "ms");
        } 
    }
}
```

---

## 2. AOP 적용

+ 공통 관심 사항 vs 핵심 관심 사항 분리

![Desktop View](/assets/img/post/spring-boot/2020-12-28-springboot-aop2.png)

---

## 2-1. Aop 코드 작성

```java
//package: hello.spring/aop
//TimeTraceAop.java

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Aspect;

@Aspect
public class TimeTraceAop {

    //Around 어노테이션을 통해 Aop를 적용할 수준을 정할 수 있습니다.
    //hellospring 패키지 이하에 전부 적용하기
    @Around("execution(* hello.hellospring..*(..))")
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        System.out.println("START: " + joinPoint.toString());
        try {
            return joinPoint.proceed();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("END: " + joinPoint.toString() + " " + timeMs + "ms");

        }
    }
}
```

---

## 2-2. Spring Bean으로 등록

+ ComponentScan 방식으로 Aop 클래스에 @Component를 추가하거나 직접 Config 클래스에 등록하면 됩니다.
+ Aop 클래스는 특별한 일을 하는 클래스이므로 `ComponentScan`보다 `직접` Config에 `Bean으로 등록`해주는 게 낫습니다. 

```java
//SpringConfig.java

@Configuration
public class SpringConfig {


    //...

    @Bean
    public TimeTraceAop timeTraceAop() {
        return new TimeTraceAop();
    }
}
```

---

## 3. AOP 동작 방식

+ **Spring은 AOP를 적용하면 `가짜 memberService(프록시)를 만듭니다.`**
+ 어디에 AOP를 적용할지를 정해주면, Spring Container에 스프링 빈을 등록할 때 가짜 스프링빈을 앞에 세워놓습니다.
+ AOP로직은 가짜 스트링빈을 통해 돌리고 AOP 로직이 끝나고 `joinPoint.proceed()`를 하면 그 때 진짜를 클래스를 호출해줍니다.
+ helloController가 호출하는 건 진짜가 아니라 프록시 서비스를 호출합니다.

1. 그래서 스프링 컨테이너가 클래스를 보고 `AOP가 적용되어야하면`, 그 클래스에 대한 프록시를 가져옵니다.
2. 그리고 프록시를 통해 AOP Logic이 다 실행되고 난 후 `joinPoint.proceed()`가 호출될 때 진짜 클래스가 호출됩니다.

![Desktop View](/assets/img/post/spring-boot/2020-12-28-springboot-aop3.png)

![Desktop View](/assets/img/post/spring-boot/2020-12-28-springboot-aop4.png)

---

+ 출처
	+ [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
