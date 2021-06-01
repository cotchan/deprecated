---
title: Spring) SpringBoot 자동 설정 원리 (@SpringBootApplication)
author: cotchan 
date: 2021-06-01 22:58:21 +0800 
categories: [Spring, Spring_Basic]
tags: [spring] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 자동설정이란

+ 스프링 부트는 스프링에서 어플리케이션을 만들 때 주로 사용하는 설정을 자동으로 설정할 수 있습니다.
+ **이 기능은 자바의 `main 진입점에 @SpringBootApplication을 붙임`으로써 사용이 가능합니다.**

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

---

## 2. @SpringBootApplication 정체

+ **위 `@SpringBootApplication` 어노테이션은 `다음 3개의 어노테이션을 붙인 효과와 동일`합니다.**

```java
@Configuration
@ComponentScan
@EnableAutoConfiguration
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

+ **스프링 부트는 @SpringBootApplication에 있는 `@ComponentScan`과 `@EnableAutoConfiguration`을 통해 `두 단계로 나누어` 스프링 부트 프로젝트의 `스프링 빈을 찾아내어 등록합니다.`**

---

## 3. @ComponentScan

+ **@CompoentScan은 `@Component`라는 어노테이션이 붙어있는 class를 빈으로 등록합니다.**

+ `@Component`
+ `@Configuration`
+ `@Repository`
+ `@Service`
+ `@Controller`
+ `@RestController`
+ 기타 등등

---

## 4. @EnableAutoConfiguration 

+ **`@EnableAutoConfiguration`은 스프링 부트에서, 스프링에서 많이 쓰이는 스프링 빈을 자동으로 컨테이너에 등록하는 역할을 하는 어노테이션.**
+ @EnableAutoConfiguration이 등록하는 빈들의 목록은 `spring-boot-autoconfigure-2.X.X.RELEASE.jar 파일에 포함`되어 있습니다.
+ 위 목록 중에는 @EnableAutoConfiguration을 붙였을 시 스프링 부트 프로젝트를 `웹 프로젝트로 만들 수 있는 기본값이 설정`되어 있습니다.

---

## 5. 이 클래스의 적정 위치

+ **@SpringBootApplication의 적정한 위치**



---

+ 출처
  + [[Spring Boot] 자동 설정 이해하기 @EnableAutoConfiguration](https://cornswrold.tistory.com/314)
