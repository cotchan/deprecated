---
title: Spring-Boot) 의존 라이브러리 살펴보기(Feat.Gradle 의존관계)
author: cotchan 
date: 2021-01-13 08:45:21 +0800 
categories: [Spring-Boot, Spring-Boot_ETC]
tags: [spring-boot] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. gradle 의존관계 보기

```bash
./gradlew dependencies —-configuration compileClasspath
```

---


## 2. 스프링 부트 라이브러리

+ **`spring-boot-starter-web`**
  + `spring-boot-starter-tomcat`: 톰캣(웹서버)
  + `spring-webmvc`: 스프링 웹 MVC

+ `spring-boot-starater-thymeleaf`: 타임리프 템플릿 엔진
+ `spring-boot-starter-data-jpa`
  + `spring-boot-starat-aop`
  + `spring-boot-starter-jdbc`
    + `HikariCP` 커넥션 풀(부트 2.0 기본)
  + hibernate + JPA: 하이버네이트 + JPA
  + spring-data-jpa: 스프링 데이터 JPA
+ **`spring-boot-starter`(공통)**: 스프링 부트 + 스프링 코어 + 로깅
  + `spring-boot`
    + `spring-core`
  + `spring-boot-starter-loggin`
    + logback, slf4j

---

## 3. 테스트 라이브러리

+ **`spring-boot-starter-test`**
  + junit: 테스트 프레임워크
  + mockito: 목 라이브러리
  + assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
  + spring-test: 스프링 통합 테스트 지원

---

## 4. 핵심 라이브러리

+ 스프링 MVC
+ 스프링 ORM
+ JPA, 하이버네이트
+ 스프링 데이터 JPA 

---

## 5. 기타 라이브러리

+ H2 데이터베이스 클라이언트
+ 커넥션 풀: 스프링 부트 기본은 HikariCP
+ WEB(thymeleaf)
+ 로깅 SLF4j & LogBack
+ 테스트


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
